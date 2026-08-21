## Auditd

Auditd is a Linux auditing tool that can be used to monitor changes and security-related events on a server or PC.

It uses rules to monitor specific files, system calls, and security-related activities. You can create rules for specific files and configure Auditd to watch them.

According to the video, local Auditd rules are stored in:

```text
/etc/audit/rules.d/*.rules
```

The content of the rules file is:

```bash
-w /etc/sudoers -p wa -k scope
-w /etc/sudoers.d -p wa -k scope

-a always,exit -F arch=b64 -S adjtimex,settimeofday -k time-change
-a always,exit -F arch=b32 -S adjtimex,settimeofday -k time-change
-a always,exit -F arch=b64 -S clock_settime -F a0=0x0 -k time-change
-a always,exit -F arch=b32 -S clock_settime -F a0=0x0 -k time-change
-w /etc/localtime -p wa -k time-change

-a always,exit -F arch=b64 -S sethostname,setdomainname -k system-locale
-a always,exit -F arch=b32 -S sethostname,setdomainname -k system-locale
-w /etc/issue -p wa -k system-locale
-w /etc/issue.net -p wa -k system-locale
-w /etc/hosts -p wa -k system-locale
-w /etc/networks -p wa -k system-locale
-w /etc/network -p wa -k system-locale
-w /etc/netplan -p wa -k system-locale

-w /etc/group -p wa -k identity
-w /etc/passwd -p wa -k identity
-w /etc/gshadow -p wa -k identity
-w /etc/shadow -p wa -k identity
-w /etc/security/opasswd -p wa -k identity
-w /etc/nsswitch.conf -p wa -k identity
-w /etc/pam.conf -p wa -k identity
-w /etc/pam.d -p wa -k identity

-w /var/log/lastlog -p wa -k logins
-w /var/run/faillock -p wa -k logins
```

After saving the rules file, run the following commands:

```bash
sudo systemctl restart auditd
sudo augenrules --load
sudo auditctl -l | head -20
sudo ausearch -k identity | tail -1
sudo aureport --summary
```

These commands restart Auditd, load the rules, display the active rules, search for events associated with the `identity` key, and display a summary report.

At this point, I have configured Auditd and tested the rules.

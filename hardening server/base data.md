# Server Hardening

## Introduction

First, I have a simple question:

**Why do we need to harden our servers? Why do we need to harden our PCs?**

Think about why you lock your car or your house before leaving. You do it for safety and to protect your belongings.

So why shouldn't we protect our data, websites, games, and other important resources?

Server hardening helps protect your system and data from unauthorized access.

In this project, I am starting to harden a Linux server that is running on my PC inside an isolated environment. This allows me to safely experiment with different security configurations.

I am following a video course to learn how to harden a Linux server.

In this part, I am learning about:

- Auditd
- Fail2ban
- SSH hardening

---

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

---

## UFW (Uncomplicated Firewall)

UFW stands for **Uncomplicated Firewall**.

UFW itself is not a firewall engine. It is a simple interface for managing firewall rules, typically through the Linux firewall stack.

With UFW, you can control which connections are allowed and which connections are denied.

This is an important security tool. For example, if an unnecessary port is open, an attacker may be able to use that exposed service to gain unauthorized access to the system.

Therefore, unnecessary ports should remain closed, especially on systems exposed to public networks.

### Basic UFW configuration

To configure the basic firewall policies, run:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default deny routed
```

These policies mean:

- Deny incoming connections by default.
- Allow outgoing connections by default.
- Do not allow the system to forward routed traffic by default.

At this point, UFW is configured to:

- Block incoming connections unless a specific rule allows them.
- Allow outgoing connections.
- Disable routed traffic by default.

However, there is one important thing we are missing:

### SSH access

At the moment, I use SSH to connect to the server through TCP port `22`. Therefore, I need to create a firewall rule that allows SSH connections:

```bash
sudo ufw allow 22/tcp comment 'SSH'
```

The comment helps identify why this port is open.

To see the rules that have been added to UFW, run:

```bash
sudo ufw show added
```

At this point, UFW knows what rules to use, but the firewall has not been enabled yet.

To activate UFW:

```bash
sudo ufw enable
```

That's all for the basic UFW configuration.

I will continue adding more hardening configurations later.
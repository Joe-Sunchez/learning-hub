# Fail2Ban

Fail2Ban is a tool that scans log files and detects suspicious behavior.

UFW gives us firewall rules, but Fail2Ban makes these rules intelligent. It can automatically detect repeated attacks, such as brute-force attempts, and temporarily ban the attacker's IP address without manual intervention.

First, type:

```bash
sudo apt install fail2ban
```

This command installs Fail2Ban.

After that, we need to start and enable the service.

To do that, type:

```bash
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
sudo systemctl status fail2ban
```

The last command will show the status of Fail2Ban.

---

### Let's Talk About Jails

A **jail** is a monitoring profile for a specific service.

A jail defines:

* What to monitor
* Which patterns indicate an attack
* What action should be taken when an attack is detected

To create a local jail configuration, type:

```bash
sudo nano /etc/fail2ban/jail.local
```

Then add the following configuration:

```text
[DEFAULT]
bantime = 600
findtime = 600
maxretry = 5
banaction = ufw


[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 5
```

For now, we have added a jail for SSH and configured it.

Now it is time to restart Fail2Ban:

```bash
sudo systemctl restart fail2ban
```

To see the status of Fail2Ban, type:

```bash
sudo fail2ban-client status
```

For more details, add `sshd` to the end of the command:

```bash
sudo fail2ban-client status sshd
```

With this command, you can see how many people have been banned and which IP addresses have been banned.

---

## Testing Fail2Ban

You can test Fail2Ban in two ways:

* You can create fake log entries.
* You can test it from another device by performing multiple failed login attempts.

To unban all banned IP addresses, type:

```bash
sudo fail2ban-client unban --all
```

Your server/PC can now detect and block brute-force attacks automatically and help keep the system safe.

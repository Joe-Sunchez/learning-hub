# Fail2Ban

this a tool (?) that scans the log file and stops the speshez behaver the ufw gives us the firewall rules but fail2ban make these rules intaligent no manual intervangent needed the attack stops it self 
fisrt type :
```bash
sudo apt install fail2ban
``` 
this command install's the fail2ban 

after that we need to statrt and enable this app 
to do that type:
```bash
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
sudo systemctl status fail2ban

```

the last command well shows the status of fail2ban 

### lets talk about the jail's
a jail is a monitoring profile for especifec service which jail defind what to watch what paterns indecate attak's and what well the respond 

to create a local jeil type :
```bash 
sudo nano /etc/fail2ban/jail.local
```
than for the content of the jail type :

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

so for now we add a jail for ssh and set config of it .
now its time to restart the fail2ban using :
```bash
sudo systemctl restart fail2ban
```

to see the status of fail2ban type :
```bash
sudo fail2ban-client status
```
to more detail add the sshd end of last command :

```bash
sudo fail2ban-client status sshd
```
on this command you can see who many peplie is banded and what is banded ip's

you can test the fail2ban by two way :
 - you can create the facke logs 
 - os you can tested by a device 

to unban pepele you can type :
```bash
sudo fail2ban-client unban --all
```

you server/pc now blocks the beotforc attake and keep it salf safe .
# hardning server 

at first a have a good question :

why we need to hardening our server ? why we need to use hardening in our PC's ?

for the first one why you lock your car door or lock your door befor going out ??? you shoud say for my safety or to keep my things safe . thats the job of a smart human . so why you dont keep your data ,your website or your games or other things that importent to you.

hardening helps you keep your PC safe and your data un acsseceble form other persons .
at here i start to hardening a server what i run it in my PC in the isolated inviroment so i can acsses it any time i want .

i am starting to whatch a video to hardening my server .
at this part i learning about auditd , fail2ban , ssh hardening . 

## auditd 

auditd is a linux tool (as fare as i know ) it help you to see the changes on your server or your PC .
this tool uses rules to secure your server/PC safe . you can set rules to a unic file and set auditd to whtch it .

as the video auditd local rules seve at the /etc/audit/rules.d/***.rules . the content of the file is :
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

than save the file and run the below commands
```bash
systemctl restart auditd.*   #this one restart the auditd
sudo augenrules --load      #this one load the rules what we rode 
sudo auditctl -l | head -20     #at here we have to see our rules 
sudo ausearch -k identity | tail -1     # we see the auditd report here 
sudo aureport --summary     # see what hapend :)

```

we run the auditd and configed it together . if you need a help use the ai for good it know's all things :)

## UFW  (Uncomplicated Fire Wall)

ufw it self not a kind of firewall , mostly it's a sempel user interface to manage the iptables/nftables .
whith the UFW you can tell system what port's can have comnecation and what port's can not .

this a very importent tool to keep server/PC safe if you open a port like 6172 . a hacker can semply use it you acsses to the terminal/CMD ,than run a command . so we have to keep our port close in the public network.


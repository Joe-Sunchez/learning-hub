# SSH Hardening 

### at first what is ssh ?

ssh is a way that you can use it to connect the server or  pc's terminal .

so hardening this service is critical for users and developer to keep :
- you data safe
- minimaize the acsses of other pepele
- connecting to target esier
- a solution to sand and resive the file 

at here we will :
- remove the password login 
- config this service
- learn how to use it 

at the moment i use a *PC (ubuntu)* and a *VM server (ubuntu-server)* so when i sey **PC** i am talking about my main PC and when i sey **server**  i am talking about my VM server :)

## Connect to Server

at PC open a terminal and type:

```bash
ssh YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```
**THIS COMMEDS ALSO RUN IN WINDOWS CMD**

type yes for the trust question and than enter your server password you will connect to the server :)

## backup

for start we want to chenge the config of ssh in out server so dont forget to get a backup of default config's 
to do that type :

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

than to see what we do , you can type :

```bash
ls -lh
```
now you have to get a list in that list you have to see these to file :

- sshd_config
- sshd_config.backup

### now we need a new keypair 


to create the keypair at PC type :

```bash
ssh-keygen
```
or you can short the genaring prosses using :

```bash 
ssh-keygen -t ed25519 -C " *** YOUR COMMENT :) *** "
```

after a enter the ssh service will ask you about the location and the passphrase you can costome it if you want or you can skip it by pressing enter.

at the end by typing :

```bash
ls -ltrh ~/.ssh/
```

you will see these file's 

```text
id_ed25519
id_ed25519.pub
```

we need the content of one of these file (the .pub one) so we type :

```bash
cat ~/.ssh/id_ed25519.pub
```

than copy the content line (Ctrl+Shift+C)

go to the server teminal and fo to .ssh dir

somtime at the server **.ssh** dir not exist you can create it by type :

```bash
mkdir -p ~/.ssh ; chmod 700 ~/.ssh
```

to craete the authorized keys file (if it's not exist) type :

```bash
nano ~/.ssh/authorized_keys
```

here you have to paste the content of id_ed25519.pub file (use Ctrl+Shift+V)

than with Ctrl+O and Ctrl+X close the nano than chenge the acess to this file using :

```bash 
chmod 600 ~/.ssh/authorizes_keys
```

now by typing the 'exit' close the server terminal in PC and try to connect to server again using : 

```bash
ssh YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

**now you have to connect the server without any password**

now we want to cheng the sshd_config file . open the config file with nano using :

```bash 
sudo nano /etc/ssh/sshd_config
```

we have to uncomment some of these configs and also chenge some of them.
i write the parts that have to uncommect and if you see the **-->** it mins changing the value .

```text
LogLevel INFO
LogingGraceTime 2m --> 60
PermitRootLogin prohibit-password --> no
MaxAuthTries 6 --> 4
MaxSessions 10 
PermitEmptyPasswords no
ClientAliveInterval 0 --> 15
ClientAliveCountMax 3 --> 15
```

apply these chenges and seve the file .than go to this file and edit it :

```bash
sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf
```

than chenge the :
 
```text
PasswordAuthentication yes --> no
```

ok seve and exit the file .

## what these changes do ?

these chenges give us :
- ditaile login (login logs)
- disconnecting any connection who unused for more than 60 sacend
- disable login as root
- limitation of password atamps befor disconnacting 
- limit's the sessions 
- blocking the accounnts who has no password
- checking the client connacton evry 15 sacend and if 3 time chacking failed disconnact the client
- blocking the authentication with password in ssh 

to check the configoration of ssh type :

```bash
sudo sshd -t
```

if nothing shows up we are good . to activate all the changes we need to re start the service using :

```bash
sudo systemctl restart sshd
```

to checking the status of the service :

```bash
sudo systemctl restart sshd
```

if its active our chenges has made and thay work. to test the configoration close the connation to server than try to connect the server without a key using :

```bash
ssh -o PubkeyAuthentication=no YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

if you get a 

```text
Permission denied
```

that's mines the configoration works now try to connect the server normaly :

```bash
ssh YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

now you have to connect emideitly 

thats all
# SSH Hardening

### At First, What Is SSH?

SSH is a protocol that allows you to connect to a server or PC and access its terminal remotely.

Hardening this service is critical for users and developers because it helps to:

* Keep your data safe
* Minimize unauthorized access
* Make secure connections to the target easier
* Provide a secure way to send and receive files

In this guide, we will:

* Disable password-based login
* Configure the SSH service
* Learn how to use SSH securely

At the moment, I use a **PC (Ubuntu)** and a **VM server (Ubuntu Server)**. So, when I say **PC**, I am talking about my main PC, and when I say **server**, I am talking about my VM server. :)

---

## Connect to the Server

On the PC, open a terminal and type:

```bash
ssh YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

**These commands also work in Windows CMD.**

Type `yes` when asked whether you want to trust the server, and then enter your server password. You will be connected to the server. :)

---

## Backup

First, we want to change the SSH configuration on our server, so don't forget to make a backup of the default configuration.

To do that, type:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Then, to check the files, you can type:

```bash
ls -lh /etc/ssh/
```

You should see these two files:

* `sshd_config`
* `sshd_config.backup`

---

### Now We Need a New Key Pair

To create an SSH key pair on the PC, type:

```bash
ssh-keygen
```

Or, you can speed up the key-generation process by using:

```bash
ssh-keygen -t ed25519 -C "YOUR COMMENT :)"
```

After pressing Enter, the SSH service will ask you about the file location and passphrase.

You can customize them if you want, or skip them by pressing Enter.

At the end, type:

```bash
ls -ltrh ~/.ssh/
```

You should see these files:

```text
id_ed25519
id_ed25519.pub
```

We need the content of the `.pub` file, so type:

```bash
cat ~/.ssh/id_ed25519.pub
```

Then copy the output line using `Ctrl+Shift+C`.

Go to the server terminal and enter the `.ssh` directory.

Sometimes the `.ssh` directory does not exist on the server. You can create it by typing:

```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
```

To create the `authorized_keys` file, if it does not already exist, type:

```bash
nano ~/.ssh/authorized_keys
```

Paste the content of the `id_ed25519.pub` file here using `Ctrl+Shift+V`.

Then press `Ctrl+O` to save and `Ctrl+X` to exit Nano.

Now change the permissions of the file:

```bash
chmod 600 ~/.ssh/authorized_keys
```

Now type:

```bash
exit
```

This will close the server session.

From the PC, try to connect to the server again:

```bash
ssh YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

**Now you should be able to connect to the server without entering the server account password.**

---

## Configure SSH

Now we want to change the `sshd_config` file.

Open the configuration file with Nano:

```bash
sudo nano /etc/ssh/sshd_config
```

We have to uncomment some of these settings and change some of their values.

I have written the settings that need to be changed below. If you see `-->`, it means the value should be changed.

```text
LogLevel INFO

LoginGraceTime 2m --> 60

PermitRootLogin prohibit-password --> no

MaxAuthTries 6 --> 4

MaxSessions 10

PermitEmptyPasswords no

ClientAliveInterval 0 --> 15

ClientAliveCountMax 3
```

Apply these changes and save the file.

Then edit this file:

```bash
sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf
```

Change:

```text
PasswordAuthentication yes
```

to:

```text
PasswordAuthentication no
```

Save and exit the file.

---

## What Do These Changes Do?

These changes provide:

* Detailed login logging
* A 60-second limit for completing the SSH login process
* Disabled root login
* A limit on failed authentication attempts
* A limit on the number of simultaneous SSH sessions
* Prevention of login using an empty password
* Periodic checking of the client connection
* Disconnection of inactive connections according to the configured keepalive settings
* Disabled password authentication for SSH

---

## Check the SSH Configuration

Before restarting SSH, check the configuration for syntax errors:

```bash
sudo sshd -t
```

If nothing is displayed, the configuration syntax is valid.

To apply the changes, restart the SSH service:

```bash
sudo systemctl restart ssh
```

To check the status of the service:

```bash
sudo systemctl status ssh
```

If the service is active, the configuration has been applied successfully.

---

## Test the Configuration

To test whether password authentication has been disabled, close the current connection to the server and try to connect without using a key:

```bash
ssh -o PubkeyAuthentication=no YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

If you get:

```text
Permission denied
```

it means password authentication is disabled and the configuration is working.

Now try to connect normally:

```bash
ssh YOUR-USER-NAME-IN-SERVER@YOUR-SERVER-IP-ADDRESS
```

You should be able to connect using your SSH key.

That's all! :)

# UFW (Uncomplicated Firewall)

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

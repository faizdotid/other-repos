# How to Change Default Login to Root on Ubuntu VPS

## Step 1: Connect to VPS
Connect to your VPS using your current user:
```bash
ssh username@your_server_ip
```

## Step 2: Set Root Password
Set a password for the root user:
```bash
sudo passwd root
```

## Step 3: Edit SSH Configuration
Open the SSH configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```

Find the line containing `PermitRootLogin` and change it to:
```
PermitRootLogin yes
```

Save the file:
* Press `CTRL + X` to exit
* Press `Y` to save
* Press `Enter` to confirm

## Step 4: Restart SSH Service
On Ubuntu, use either of these commands:
```bash
sudo systemctl restart ssh
```
or
```bash
sudo service ssh restart
```

## Step 5: Login as Root
You can now log in directly as root:
```bash
ssh root@your_server_ip
```

## Security Recommendations
1. Use strong passwords or SSH keys
2. Consider using a regular user with sudo privileges
3. Set up fail2ban for protection against brute force attacks
4. Enable SSH key authentication and disable password login

**Note**: Enabling root login directly is generally not recommended for security reasons. Consider implementing the security recommendations above instead.

# Cheat Sheet: How to Secure Your Ubuntu OS

## Keep Ubuntu Updated:
Regularly update your system to ensure you have the latest security patches.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt dist-upgrade
sudo apt autoremove
```

## Enable UFW Firewall:
Set up and enable the Uncomplicated Firewall (UFW) to protect your system.

```bash
sudo ufw enable
sudo ufw status verbose
sudo ufw allow ssh
```

## Secure SSH:
1. Edit the SSH configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```
2. Disable root login and change the default SSH port:
   - Set `PermitRootLogin` to `no`
   - Change `Port` to a non-default value
3. Restart the SSH service:
```bash
sudo systemctl restart ssh
```

## Install and Configure Fail2Ban:
Fail2Ban monitors your logs for suspicious activity and bans hosts that show malicious signs -- such as repeated failed login attempts.

```bash
sudo apt install fail2ban
sudo systemctl enable fail2ban
```

## Disable Unused Services:
Turn off services you don't use to reduce attack surfaces.

```bash
sudo systemctl disbale service_name
```

## Use Secure Password Policies:
1. Install the libpam-cracklib package:
```bash
sudo apt install libpam-cracklib
```
2. Edit the password policy configuration:
```bash
sudo nano /etc/pam.d/common-password
```
3.  Add the following line to enforce strong passwords:
```bash
password requisite pam_cracklib.so retry=3 minlen=12 difok=3\
```

## Audit Security with Lynis:
Use Lynis to perform a security audit of your system.
```bash
sudo apt install lynis
sudo lynis audit system
```

## Disable Guest Account:
Prevent unauthorized access by disabling the guest account.
```bash
sudo nano /etc/lightdm/lightdm.conf
```
Add the following lines:
```bash
[SeatDefaults]
allow-guest=false
```
Restart the display manager:
```bash
sudo systemctl restart lightdm
```

## Enable Automatic Security Updates:
Ensure critical security updates are installed automatically.
```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

## Use AppArmor for Application Security:
AppArmor provides mandatory access control for applications.
1.  Ensure AppArmor is installed and enabled:
```bash
sudo apt install apparmor apparmor-utils
sudo systemctl enable apparmor
sudo systemctl start apparmor
```
2.  Check AppArmor status:
```bash
sudo aa-status
```

## Secure Shared Memory:
Restrict access to shared memory to prevent exploits.
1.  Eidt the fstab file:
```bash
sudo nano /etc/fstab
```
2.  Add the following line:
```bash
tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0
```
3.  Remount the shared memory:
```bash
sudo mount -o remount /run/shm
```

## Use a Host-Based Intrusion Detection System (HIDS):
Install and configure a HIDS like AIDE to monitor file changes.
1.  Install AIDE:
```bash
sudo apt install aide
```
2.  Initialize the AIDE database:
```bash
sudo aideinit
```
3.  Replace the default database with the new one:
```bash
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db
```
4.  Run a manual check:
```bash
sudo aide --check
```

## Limit User Privileges:
Use the usermod command to ensure users have only the privileges they need.
```bash
sudo usermod -L username # Lock a user account
sudo usermod -s /usr/sbin/nologin username # Disable shell access
```

## Configure System Logs:
Ensure logs are properly configured and monitored.
1.  Install and configure rsyslog:
```bash
sudo apt install rsyslog
sudo systemctl enable rsyslog
```
2.  View logs:
```bash
sudo less /var/log/syslog
```

## Encrypt Sensitive Data:
Use LUKS to encrypt sensitive partitions.
1.  Install LUKS:
```bash
sudo apt install cryptsetup
```
2.  Encrypt a partition:
```bash
sudo cryptsetup luksFormat /dev/sdX
sudo cryptsetup open /dev/sdX encrypted_partition
```
3.  Mount the encrypted partitions:
```bash
sudo mount /dev/mapper/encrypted_partition /mnt
```

==================================

These additional tips will further enhance the security of your Ubuntu OS.

#### Feel free to contribute to this cheat sheet by forking the repository and submitting pull requests!

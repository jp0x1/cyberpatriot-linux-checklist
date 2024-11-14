# Forensic Questions
1. just do what they ask
2. always delete / do something about the stuff u gotta do in forensics question

guaranteed extra points!

# Remove Unauthorized Users

```bash
sudo deluser --remove-home username
```

# Remove Users From Sudo Group or Group in general

```bash
sudo deluser username groupname
```

# Create User Group

```bash
sudo groupadd groupname
```

# Add User to Group

```bash
sudo usermod -aG groupname username
```

# Change Insecure Password

```bash
sudo passwd username
```

carefully note down the password

# Set Default Maximum Password Age
> /etc/login.defs 
```bash
PASS_MAX_DAYS 30
PASS_MIN_DAYS 7
PASS_WARN_AGE 12
```
# Password Strength (LAST THING TO DO SO YOU DON'T FUCK UP)
```bash
apt-get install libpam-cracklib
```
> /etc/pam.d/common-password

```bash
password	required	pam_unix.so obscure sha512 remember=12 use_authtok
password	required	pam_cracklib.so retry=3 minlen=13 difok=4 dcredit=-1 ucredit=-1 ocredit=-1 lcredit=-1 maxrepeat=3
```

> /etc/pam.d/common-auth
```bash
auth	required	pam_tally2.so deny=5 audit unlock_time=1800 onerr=fail even_deny_root
```

> /etc/default/useradd
```bash
EXPIRE=30
INACTIVE=30
```

# Lock Root User

```bash
passwd -l root
```

# GUFW

```bash
sudo apt install gufw
```

Status: on

Incoming: deny

Outgoing: allow

Logging high

# Remove Programs

List installed services first:

```bash
dpkg --list
apt list --installed
```

Then remove:

```bash
sudo apt remove program_name
sudo apt autoremove
```

List of services to remove to get some extra points (if not explicitly mentioned in README):

- apache2
- nginx
- OpenArena
- SQL(?)
- FTP (?)

Any network sniffers + network stuff + games SHOULD BE REMOVED!

# Remove Prohibited Files

```bash
find /home/ -type f \( -name "*.mp3" -o -name "*.mp4" \)
```
replace with `.txt` or whatever

# Stop non-essential / bad services
- remote connection services
- file transfer services
- mailing services

```bash
sudo systemctl stop service_name
```

# Automatically Updates Daily 

1. Open the Update Manager from the Menu.
2. Click on Edit > Preferences.
3. Go to the Auto-Updates tab.
4. Enable Automatic Refresh of Package Information.
5. Set the Refresh Frequency to "Daily" or your preferred frequency.

# Update system

```bash
sudo apt update
```

# Update software

```bash
sudo apt upgrade
```

# SSH
> /etc/ssh/sshd_config
```bash
Protocol 2
LogLevel VERBOSE
X11Forwarding no
MaxAuthTries 4
IgnoreRhosts yes
HostbasedAuthentication no
PermitRootLogin no
PermitEmptyPasswords no
```

# FTP 
> /etc/vsftpd.conf

Make sure SSL stuff

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem
```

Config stuff

```bash
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH
rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem

userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO

chroot_local_user=YES
allow_writeable_chroot=YES

anonymous_enable=NO
```
Create list of allowed users: 

> /etc/vsftpd.userlist

Make sure to delete unauthorized files in the FTP server

# PostgresSQL 

> /etc/postgresql/...

> pg_hba.conf
- turn off stuff that is not needed(?)

> postgresql.conf
```bash
listen_address = 'localhost' #Only if the server is NOT needed by an external service.
ssl_cert_file = '/etc/ssl/certs/postgres.crt'
ssl_key_file = '/etc/ssl/private/postgres.key'
row_security = on
log_connections = on
```

> pg_ident.conf
- remain empty unless specified by readme

# Ignore ICMP Echo Requests

```bash
sudo sysctl -w net.ipv4.icmp_echo_ignore_all=1
```

# LIGHTDM config
> /etc/lightdm/lightdm.conf
```bash
allow-guest=false
greeter-hide-users=true
greeter-show-manual-login=true
autologin-user=none
```
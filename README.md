# home lab sysadmin project

this is the first project in my personal homelab.  
I wanted to really understand how linux works behind the scenes  
so I went hands on with user management, permissions, updates, networking, and real troubleshooting.  
i didnt copy or paste this lab. I hit real errors, fixed them, and learned how a system behaves when something breaks.

---

## what I completed

1. checked basic system information and explored the file structure  
2. updated and upgraded packages  
3. fixed apt lock errors after interrupting an upgrade  
4. created a new user and configured sudo access  
5. worked with permissions and file ownership  
6. explored networking commands like ip, ping, and hostname  
7. documented the entire process  

---
### stage one   basic system info

I started by confirming who I was logged in as and what system I was working on.

```bash
whoami
hostnamectl
uname -a
pwd 
```
This helped me confirm I was inside my VM and understand my starting point.

Key details from the output:
```bash
user:       phantomlab
hostname:   phantomlab-IdeaPad-1-15IRU7
os:         Ubuntu 24.04.1 LTS
kernel:     Linux 6.14.0-33-generic (x86_64)
hardware:   Lenovo IdeaPad 1 15IRU7 laptop
firmware:   MCN30WW (2024-10-28)
home dir:   /home/phantomlab
```
This told me I was on my Linux VM with the right user and confirmed the exact Ubuntu and kernel version before making any changes.

## stage two   update and upgrade

I refreshed the system package list and installed newer versions:

```
sudo apt update
sudo apt upgrade
```

because I interrupted a previous upgrade, I hit **apt lock errors**.  
linux uses lock files to stop two package processes from running at once.

to fix it:

```
ps aux | grep apt
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/cache/apt/archives/lock
sudo dpkg --configure -a
```

After re-running update and upgrade, everything worked normally.

I also learned that:

```
0 upgraded, 0 newly installed, 0 to remove, and 1 not upgraded
```

isn’t an error. ubuntu sometimes holds back packages if dependencies aren’t ready.

optional cleanup:

```
sudo apt autoremove
```

---

## stage three   user and sudo configuration

I created a new user:

```
sudo adduser labuser
```

I got a **BAD PASSWORD** warning because my password was too simple  
but linux lets you retry as long as you retype correctly.

after filling in optional metadata fields, the system added the user to the default group:

**users**

then I added sudo privileges:

```
sudo usermod -aG sudo labuser
```

and switched into the new account:

```
su - labuser
whoami
```

---

## stage four   permissions and ownership

I created a test directory:

```
mkdir testperm
cd testperm
```

inside it, I made a file:

```
echo "hello world" > file1.txt
ls -l
```

then restricted permissions:

```
chmod 600 file1.txt
```

this changed permissions to:

```
-rw-------
```

meaning:

- owner can read and write  
- no access for group or others  

I practiced changing ownership:

```
sudo chown root file1.txt
sudo chown labuser file1.txt
```

---

## stage five   networking basics

I checked my network interfaces:

```
ip a
```

here I found my IPv4 address:  
**192.168.1.97**

I tested internet connectivity:

```
ping -c 4 google.com
```

`-c 4` sends exactly four pings.

I used a cleaner command to see only my machine’s IP:

```
hostname -I
```

uppercase I is important — lowercase i is different.

---

## errors I faced and how I fixed them

### apt lock errors  
caused by interrupting an upgrade  
fixed by removing lock files and reconfiguring dpkg

### password warnings  
too short or mismatched passwords  
fixed by retrying until accepted

### su not switching  
happened before proper sudo configuration  
fixed after using the correct usermod command

### permission confusion  
got clearer after checking `ls -l` before and after chmod

### trouble reading ip output  
solved by using `hostname -I` for clean IP listings

---

## what I learned

this project showed me what real system behaviour looks like.  
I learned how linux protects processes with locks, how permissions actually work,  
and how to troubleshoot calmly by reading errors and fixing them step by step.

it made me more confident in sysadmin, security, and homelab work.

---

## next steps

- set up SSH access  
- configure a firewall with ufw  
- explore systemctl and services  
- create a multi user environment  
- expand this project into a full homelab series  


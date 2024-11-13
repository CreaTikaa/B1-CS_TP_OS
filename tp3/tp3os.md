# TP 3  
### I. Utilisateurs  
**1. Liste des users**  

1/
```
crea@tpos:~$ cat /etc/passwd | grep crea
crea:x:1000:1000:crea,,,:/home/crea:/bin/bash
```
2/
```
crea@tpos:~$ cat /etc/passwd | grep -e crea -e root
root:x:0:0:root:/root:/bin/bash
crea:x:1000:1000:crea,,,:/home/crea:/bin/bash
```
3/
```
crea@tpos:~$ cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:crea
floppy:x:25:crea
tape:x:26:
sudo:x:27:crea
audio:x:29:pulse,crea
dip:x:30:crea
www-data:x:33:
backup:x:34:
operator:x:37:
list:x:38:
irc:x:39:
src:x:40:
shadow:x:42:
utmp:x:43:
video:x:44:crea
sasl:x:45:
plugdev:x:46:crea
staff:x:50:
games:x:60:
users:x:100:crea
nogroup:x:65534:
systemd-journal:x:999:
systemd-network:x:998:
crontab:x:101:
input:x:102:
sgx:x:103:
kvm:x:104:
render:x:105:
netdev:x:106:crea
systemd-timesync:x:997:
messagebus:x:107:
_ssh:x:108:
ssl-cert:x:109:
avahi-autoipd:x:110:
bluetooth:x:111:crea
avahi:x:112:
lpadmin:x:113:crea
pulse:x:114:
pulse-access:x:115:
scanner:x:116:saned,crea
saned:x:117:
lightdm:x:118:
polkitd:x:996:
rtkit:x:119:
colord:x:120:
crea:x:1000:
marmotte:x:1001:
```
4/
```
crea@tpos:~$ cat /etc/passwd | cut -d":" -f1,6 | grep -e crea -e root
root:/root
crea:/home/crea
```
**2. Hash des passwords**  
1/
```
crea@tpos:~$ sudo cat /etc/shadow | grep crea
[sudo] password for crea: 
crea:$y$j9T$R9lfI7ISVv4LBzmg3Tr121$MtYnAp4OMgiIV0ITvQojlB7gtcvUE7ih7eNJY2xNuL8:20033:0:99999:7:::
```
**3. Sudo**  
1/
```
crea@tpos:~$ sudo groupadd stronk_admins
```
2/
```
crea@tpos:~$ sudo useradd -m imbob
crea@tpos:~$ sudo passwd imbob
New password: 
Retype new password: 
passwd: password updated successfully
```
3/
```
crea@tpos:~$ cat /etc/passwd | grep imbob
imbob:x:1002:1003::/home/imbob:/bin/sh
```
4/
```
crea@tpos:~$ sudo cat  /etc/shadow | grep imbob
imbob:$y$j9T$/eCYtZhhn/cCE3SxeI1a5/$mQ.XdJWBqaSXNbiY/6CXk4flUbQdzTBaEORXYlYle.3:20039:0:99999:7:::
```
5/
```
crea@tpos:~$ sudo cat  /etc/group | grep imbob
imbob:x:1003:
```
6/
```
crea@tpos:~$ sudo useradd imnotbobsorry
crea@tpos:~$ sudo passwd imnotbobsorry 
New password: 
Retype new password: 
passwd: password updated successfully
```
7/
```
sudo visudo
%stronk_admins ALL=(ALL) NOPASSWD: /usr/bin/apt, /usr/bin/apt-get
```
8/
```
crea@tpos:/home$ sudo mkdir goodguy
crea@tpos:/home$ sudo usermod -d /home/goodguy imbob 
crea@tpos:/home$ cat /etc/passwd | grep imbob
imbob:x:1002:1003::/home/goodguy:/bin/sh
```
9/
```
crea@tpos:/home$ sudo mkdir badguy
crea@tpos:/home$ sudo usermod -d /home/badguy imnotbobsorry 
crea@tpos:/home$ cat /etc/passwd | grep imnotbobsorry
imnotbobsorry:x:1003:1004::/home/badguy:/bin/sh
```
10/
```
crea@tpos:/home$ ls -l | grep goodguy
drwxr-xr-x  2 root     root     4096 Nov 12 11:41 goodguy

crea@tpos:/home$ sudo chown imbob:imbob goodguy -R
crea@tpos:/home$ ls -l | grep goodguy
drwxr-xr-x  2 imbob    imbob    4096 Nov 12 11:41 goodguy

crea@tpos:/home$ sudo chown imnotbobsorry:imnotbobsorry badguy/ -R
crea@tpos:/home$ ls -l | grep badguy
drwxr-xr-x  2 imnotbobsorry imnotbobsorry 4096 Nov 12 11:46 badguy

```
11/
```
crea@tpos:~$ su - imbob
Password: 
$ pwd
/home/goodguy
$ sudo cat /etc/shadow | grep imbob
[sudo] password for imbob: 
imbob:$y$j9T$/eCYtZhhn/cCE3SxeI1a5/$mQ.XdJWBqaSXNbiY/6CXk4flUbQdzTBaEORXYlYle.3:20039:0:99999:7:::  

crea@tpos:~$ su - imnotbobsorry 
Password: 
$ pwd
/home/badguy
$ sudo cat/etc/shadow
[sudo] password for imnotbobsorry: 
imnotbobsorry is not in the sudoers file.
$ sudo apt update
[sudo] password for imnotbobsorry: 
imnotbobsorry is not in the sudoers file.

crea@tpos:~$ sudo usermod -a -G stronk_admins imnotbobsorry 
crea@tpos:~$ su - imnotbobsorry 
Password: 
$ sudo echo meow
[sudo] password for imnotbobsorry: 
Sorry, user imnotbobsorry is not allowed to execute '/usr/bin/echo meow' as root on tpos.bababoy.
$ sudo apt update
Get:1 http://security.debian.org/debian-security bookworm-security InRelease [48.0 kB]
etc, etc….
```

### II. Processes
**1. Jouer avec la commande ps**  
1/
```
crea@tpos:~$ ps -ef | grep bash
crea        1182    1157  0 11:06 pts/0    00:00:00 bash
crea        1186    1157  0 11:06 pts/1    00:00:00 bash
crea        5218    1182  0 12:12 pts/0    00:00:00 grep bash
```
2/
```
crea@tpos:~$ ps -ef | grep crea
crea         705       1  0 11:05 ?        00:00:00 /lib/systemd/systemd --user
crea         706     705  0 11:05 ?        00:00:00 (sd-pam)
crea         722     705  3 11:05 ?        00:02:08 /usr/bin/pulseaudio --daemonize=no --log-target=journal
crea         723     705  0 11:05 ?        00:00:00 /usr/bin/gnome-keyring-daemon --foreground --components=pkcs11,secrets --control-directory=/run/user/1000/keyring
crea         730     705  0 11:05 ?        00:00:00 /usr/bin/dbus-daemon --session --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
crea         731     700  0 11:05 ?        00:00:00 xfce4-session
crea         779     731  0 11:05 ?        00:00:00 /usr/bin/ssh-agent x-session-manager
crea         789     705  0 11:05 ?        00:00:00 /usr/libexec/at-spi-bus-launcher
crea         795     789  0 11:05 ?        00:00:00 /usr/bin/dbus-daemon --config-file=/usr/share/defaults/at-spi2/accessibility.conf --nofork --print-address 11 --address=unix:path=/run/user/1000/at-spi/bus_0
crea         805     705  0 11:05 ?        00:00:00 /usr/libexec/at-spi2-registryd --use-gnome-session
crea         815     705  0 11:05 ?        00:00:00 /usr/bin/gpg-agent --supervised
crea         817     731  0 11:05 ?        00:00:16 xfwm4 --display :0.0 --sm-client-id 2e9488bd2-63ef-4cd1-9079-cc70fb75737c
crea         820     705  0 11:05 ?        00:00:00 /usr/libexec/gvfsd
crea         832     731  0 11:05 ?        00:00:00 xfsettingsd --display :0.0 --sm-client-id 271bbff4a-a0be-4dde-9d42-8d7f364fbcce
crea         841     731  0 11:05 ?        00:00:08 xfce4-panel --display :0.0 --sm-client-id 21bf5bd07-fa28-495f-9414-11926b482453
crea         850     731  0 11:05 ?        00:00:02 xfdesktop --display :0.0 --sm-client-id 204ff5ff9-4f66-49f6-ac17-076027f65d07
crea         853     731  0 11:05 ?        00:00:00 xfce4-power-manager --restart --sm-client-id 21d63e4da-e071-4175-8d3f-73c00325642a
crea         857     841  0 11:05 ?        00:00:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libsystray.so 6 16777228 systray Status Tray Plugin Provides status notifier items (application indicators) and legacy systray items
crea         858     841  0 11:05 ?        00:00:01 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libpulseaudio-plugin.so 8 16777229 pulseaudio PulseAudio Plugin Adjust the audio volume of the PulseAudio sound system
crea         859     841  0 11:05 ?        00:00:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libxfce4powermanager.so 9 16777230 power-manager-plugin Power Manager Plugin Display the battery levels of your devices and control the brightness of your display
crea         860     841  0 11:05 ?        00:00:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libnotification-plugin.so 10 16777231 notification-plugin Notification Plugin Notification plugin for the Xfce panel
crea         867     731  0 11:05 ?        00:00:00 /usr/bin/python3 /usr/share/system-config-printer/applet.py
crea         868     731  0 11:05 ?        00:00:00 /usr/lib/policykit-1-gnome/polkit-gnome-authentication-agent-1
crea         869     705  0 11:05 ?        00:00:00 /usr/libexec/gvfs-udisks2-volume-monitor
crea         870     705  0 11:05 ?        00:00:00 /usr/lib/x86_64-linux-gnu/xfce4/notifyd/xfce4-notifyd
crea         873     731  0 11:05 ?        00:00:00 nm-applet
crea         883     731  0 11:05 ?        00:00:00 xiccd
crea         895     820  0 11:05 ?        00:00:00 /usr/libexec/gvfsd-trash --spawner :1.14 /org/gtk/gvfs/exec_spaw/0
crea         901     731  0 11:05 ?        00:00:00 light-locker
crea         914     705  0 11:05 ?        00:00:00 /usr/libexec/gvfsd-metadata
crea         937     705  0 11:05 ?        00:00:00 /usr/libexec/dconf-service
crea         944     841  0 11:05 ?        00:00:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libactions.so 14 16777232 actions Action Buttons Log out, lock or other system actions
crea        1157       1  0 11:06 ?        00:00:13 xfce4-terminal
crea        1182    1157  0 11:06 pts/0    00:00:00 bash
crea        1186    1157  0 11:06 pts/1    00:00:00 bash
crea        1197       1  6 11:06 ?        00:04:16 /usr/bin/x-www-browser
crea        1245    1197  0 11:06 ?        00:00:00 /usr/lib/firefox-esr/firefox-esr -contentproc -parentBuildID 20241021193311 -prefsLen 28410 -prefMapSize 248423 -appDir /usr/lib/firefox-esr/browser {d8b4509e-89b2-420c-bc8b-10c3cd7bb565} 1197 true socket
crea        1248     705  0 11:06 ?        00:00:00 /usr/libexec/xdg-desktop-portal
crea        1253     705  0 11:06 ?        00:00:00 /usr/libexec/xdg-document-portal
crea        1256     705  0 11:06 ?        00:00:00 /usr/libexec/xdg-permission-store
crea        1270     705  0 11:06 ?        00:00:00 /usr/libexec/xdg-desktop-portal-gtk
crea        1288    1197  0 11:06 ?        00:00:03 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 1 -isForBrowser -prefsLen 28551 -prefMapSize 248423 -jsInitLen 234780 -parentBuildID 20241021193311 -greomni /usr/lib/firefox-esr/omni.ja -appomni /usr/lib/firefox-esr/browser/omni.ja -appDir /usr/lib/firefox-esr/browser {8259c306-7b07-4515-ad2d-ba0f27b7d271} 1197 true tab
crea        1342    1197  0 11:06 ?        00:00:01 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 2 -isForBrowser -prefsLen 34228 -prefMapSize 248423 -jsInitLen 234780 -parentBuildID 20241021193311 -greomni /usr/lib/firefox-esr/omni.ja -appomni /usr/lib/firefox-esr/browser/omni.ja -appDir /usr/lib/firefox-esr/browser {66c268de-d46e-4d1a-865f-fb601f54f9ca} 1197 true tab
crea        1396    1197  0 11:06 ?        00:00:00 /usr/lib/firefox-esr/firefox-esr -contentproc -parentBuildID 20241021193311 -sandboxingKind 0 -prefsLen 34335 -prefMapSize 248423 -appDir /usr/lib/firefox-esr/browser {424b29c8-e286-449c-befc-1c303413904e} 1197 true utility
crea        1404    1197  0 11:06 ?        00:00:38 /usr/lib/firefox-esr/firefox-esr -contentproc -childID 3 -isForBrowser -prefsLen 31441 -prefMapSize 248423 -jsInitLen 234780 -parentBuildID 20241021193311 -greomni /usr/lib/firefox-esr/omni.ja -appomni /usr/lib/firefox-esr/browser/omni.ja -appDir /usr/lib/firefox-esr/browser {2a32cd74-9582-4d1f-85ec-e1bbb191db72} 1197 true tab
crea        1535     705  0 11:07 ?        00:00:00 /usr/bin/Thunar --daemon
crea        1547     705  0 11:07 ?        00:00:00 /usr/lib/libreoffice/program/oosplash --writer file:///home/crea/Desktop/tp3os
crea        1563    1547  0 11:07 ?        00:00:27 /usr/lib/libreoffice/program/soffice.bin --writer file:///home/crea/Desktop/tp3os --splash-pipe=5
crea        2016       1  0 11:17 ?        00:00:00 /usr/lib/speech-dispatcher-modules/sd_espeak-ng /etc/speech-dispatcher/modules/espeak-ng.conf
crea        2021       1  0 11:17 ?        00:00:00 /usr/lib/speech-dispatcher-modules/sd_dummy /etc/speech-dispatcher/modules/dummy.conf
crea        2024       1  0 11:17 ?        00:00:00 /usr/bin/speech-dispatcher --spawn --communication-method unix_socket --socket-path /run/user/1000/speech-dispatcher/speechd.sock
crea        2198    1197  0 11:20 ?        00:00:01 /usr/lib/firefox-esr/firefox-esr -contentproc -parentBuildID 20241021193311 -prefsLen 34631 -prefMapSize 248423 -appDir /usr/lib/firefox-esr/browser {d3737eb3-4b7f-4e7c-8735-87eb7ab68053} 1197 true rdd
crea        5183     705  0 12:11 ?        00:00:00 /usr/lib/x86_64-linux-gnu/xfce4/xfconf/xfconfd
crea        5281    1182  0 12:14 pts/0    00:00:00 ps -ef
crea        5282    1182  0 12:14 pts/0    00:00:00 grep crea
```
3/
```
crea@tpos:~$ ps aux --sort=-rss | head -n 6 | tail -n 5 | tr -s ' ' | cut -d ' ' -f 11,6
366000 /usr/bin/x-www-browser
283556 /usr/lib/firefox-esr/firefox-esr
174884 /usr/lib/firefox-esr/firefox-esr
169072 /usr/lib/firefox-esr/firefox-esr
165816 /usr/lib/firefox-esr/firefox-esr
```
4/
```
crea@tpos:~$ ps -ef | grep sshd
root         614       1  0 11:05 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
crea        6254    1182  0 12:28 pts/0    00:00:00 grep sshd
```
5/
```
crea@tpos:~$ ps -ef --sort pid | head -2
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 11:05 ?        00:00:00 /sbin/init
```
**2. Parent, enfant, et meurtre**  
1/
```
crea@tpos:~$ ps -ef | grep bash
crea        1182    1157  0 11:06 pts/0    00:00:00 bash
```
2/
```
crea@tpos:~$ ps -p 1157
    PID TTY          TIME CMD
   1157 ?        00:00:17 xfce4-terminal
```
3/
```
crea@tpos:~$ ps -ef | grep sleep
crea        7594    1186  0 13:00 pts/1    00:00:00 sleep 9999
```
4/
```
crea@tpos:~$ exit
logout
Connection to 192.168.56.20 closed.
PS C:\Users\creat> ssh crea@192.168.56.20
crea@192.168.56.20's password:
Linux tpos 6.1.0-26-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.112-1 (2024-09-30) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Nov 13 08:14:58 2024 from 192.168.56.3
crea@tpos:~$ ps -ef | grep sleep
crea        1547       1  0 08:15 ?        00:00:00 sleep 9999
```
### III. Services
**2. Analyser un service existant**  
1/
```
crea@tpos:~$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2024-11-13 08:50:31 CET; 34min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 600 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 611 (sshd)
      Tasks: 1 (limit: 2284)
     Memory: 5.6M
        CPU: 112ms
     CGroup: /system.slice/ssh.service
             └─611 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```
2/
```
```
3/
```
crea@tpos:~$ sudo ss -lntp | grep ssh
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=611,fd=3)) 
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=611,fd=4)) 
```
4/
```
crea@tpos:~$ sudo journalctl -xe -u ssh
~
~
Nov 13 08:50:30 tpos systemd[1]: Starting ssh.service - OpenBSD Secure Shell se>
░░ Subject: A start job for unit ssh.service has begun execution
░░ Defined-By: systemd
░░ Support: https://www.debian.org/support
░░ 
░░ A start job for unit ssh.service has begun execution.
░░ 
░░ The job identifier is 120.
Nov 13 08:50:31 tpos sshd[611]: Server listening on 0.0.0.0 port 22.
Nov 13 08:50:31 tpos sshd[611]: Server listening on :: port 22.
Nov 13 08:50:31 tpos systemd[1]: Started ssh.service - OpenBSD Secure Shell ser>
░░ Subject: A start job for unit ssh.service has finished successfully
░░ Defined-By: systemd
░░ Support: https://www.debian.org/support
░░ 
░░ A start job for unit ssh.service has finished successfully.
░░ 
░░ The job identifier is 120.
Nov 13 09:24:56 tpos sshd[2175]: Accepted password for crea from 192.168.56.3 p>
Nov 13 09:24:56 tpos sshd[2175]: pam_unix(sshd:session): session opened for use>
Nov 13 09:24:56 tpos sshd[2175]: pam_env(sshd:session): deprecated reading of u>
```
**3. Modification du service**  
*A. Configuration du service SSH*  
1/
```
crea@tpos:~$ ls -l /etc/ssh | grep config
-rw-r--r-- 1 root root   1650 Jun 22 21:38 ssh_config
drwxr-xr-x 2 root root   4096 Jun 22 21:38 ssh_config.d
-rw-r--r-- 1 root root   3223 Jun 22 21:38 sshd_config
drwxr-xr-x 2 root root   4096 Jun 22 21:38 sshd_config.d
```
2/
```
crea@tpos:~$ echo $RANDOM
32702
crea@tpos:/etc/ssh$ cat sshd_config | grep Port
Port 32702
#GatewayPorts no
```
3/
```
Connection to 192.168.56.20 closed.
PS C:\Users\creat> ssh crea@192.168.56.20
ssh: connect to host 192.168.56.20 port 22: Connection refused
PS C:\Users\creat> ssh crea@192.168.56.20 -p 32702
crea@192.168.56.20's password:
Linux tpos 6.1.0-26-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.112-1 (2024-09-30) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Nov 13 09:51:52 2024 from 192.168.56.3
crea@tpos:~$
```
Bonus : 
```
PermitEmptyPasswords no
PermitRootLogin no
AllowUsers crea
```
Nécessite plus de configuration : 
```
PasswordAuthentication no
PublicKeyAuthentication yes
```
*B. Le service en lui-même*  
4/
```
crea@tpos:~$ sudo find / -name "ssh.service"
/usr/lib/systemd/system/ssh.service
```
5/
```
crea@tpos:/usr/lib/systemd/system$ cat ssh.service | grep ExecStart=
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
```
**4. Créez votre propre service**
6/
```
crea@tpos:~$ which python3
/usr/bin/python3
```
7/
```
crea@tpos:/etc/systemd/system$ sudo touch meow_web.service
crea@tpos:/etc/systemd/system$ sudo nano meow_web.service
crea@tpos:/etc/systemd/system$ sudo systemctl daemon-reload
crea@tpos:/etc/systemd/system$ sudo systemctl start meow_web
crea@tpos:/etc/systemd/system$ systemctl status meow_web.service 
● meow_web.service - Super serveur web MEOW
     Loaded: loaded (/etc/systemd/system/meow_web.service; disabled; preset: en>
     Active: active (running) since Wed 2024-11-13 10:24:14 CET; 5s ago
   Main PID: 5269 (python3)
      Tasks: 1 (limit: 2284)
     Memory: 8.9M
        CPU: 33ms
     CGroup: /system.slice/meow_web.service
             └─5269 /usr/bin/python3 -m http.server 8888
```
8/
```
crea@tpos:~$ ps -ef | grep http.server
root        5269       1  0 10:24 ?        00:00:00 /usr/bin/python3 -m http.server 8888
```
9/
```
crea@tpos:~$ sudo ss -lntp | grep python
LISTEN 0      5            0.0.0.0:8888       0.0.0.0:*    users:(("python3",pid=5269,fd=3))
```

10/
```
crea@tpos:~$ sudo systemctl enable meow_web.service 
Created symlink /etc/systemd/system/multi-user.target.wants/meow_web.service → /etc/systemd/system/meow_web.service.
``













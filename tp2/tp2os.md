# TP 2

### I. Gameshell

### II. Files and Users

**1. Fichiers**  

*A. Find Me*

1/
```
cat /etc/passwd
cd /home/crea
```
2/
```
ls -l
total 36
drwxr-xr-x 2 crea crea 4096 Nov  7 09:27 Desktop
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Documents
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Downloads
drwxr-xr-x 2 crea crea 4096 Nov  6 13:01 gameshell
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Music
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Pictures
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Public
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Templates
drwxr-xr-x 2 crea crea 4096 Nov  6 11:49 Videos
```
3/
```
cd /etc/ssh/
ls
sshd_config
```
4/
```
root@tpos:/var/log# cd /var/log/
root@tpos:/var/log# ls
alternatives.log  dpkg.log	  lastlog  speech-dispatcher  Xorg.1.log.old
apt		  faillog	  lightdm  wtmp
boot.log	  fontconfig.log  private  Xorg.0.log
btmp		  installer	  README   Xorg.0.log.old
cups		  journal	  runit    Xorg.1.log
```

**2. Users**

*A. Nouveau user*

1/
```
useradd -m -d /home/papier_alu marmotte
passwd marmotte
```

*B. Infos enregistrées par le système*

1/
```
crea@tpos:~$ cat /etc/passwd | grep marmotte
marmotte:x:1001:1001::/home/marmotte:/bin/sh
```
2/
```
sudo cat /etc/shadow | grep marmotte
[sudo] password for crea: 
marmotte:$y$j9T$Usamb3cOCYkYLkWksBilH1$MWlNEiUt1rpogv/s2I9Uy2xVjkPRlvigGgxaGiwJpT9:20034:0:99999:7:::
```

*D. Connexion sur le nouvel utilisateur*

1/
```
exit
ssh marmotte@192.168.56.20
chocolat
ls
ls: cannot open directory '/home/crea': Permission denied
```

### III. Programmes et paquets
**1. Programmes et processus**  
*A. Run then kill*

1/
```
sleep 1000
ps -A | grep sleep
   1712 pts/0    00:00:00 sleep
```
2/
```
kill 1712
```

*B. Tâche de fond*

1/
```
sleep 1000&
```
2/
```
ps -e | grep sleep
   3043 pts/1    00:00:00 sleep
```

*C. Find paths*

1/
```
find / -type f -name ".bashrc"
/usr/bin/sleep
/usr/lib/klibc/bin/sleep
```
2/
```
find / -type f -name ".bashrc"
/home/crea/gameshell/gameshell/World/.bashrc
/home/crea/.bashrc
/home/papier_alu/.bashrc
/etc/skel/.bashrc
```

*D. La variable PATH*

1/
```
crea@tpos:~$ which sleep
/usr/bin//sleep
crea@tpos:~$ which ssh
/usr/bin//ssh
crea@tpos:~$ which ping
/usr/bin//ping
crea@tpos:~$ echo $PATH
/usr/bin/:/usr/bin/:/usr/bin/grep:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

**2. Paquets**

1/
```
crea@tpos:~$ sudo apt install git
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
git is already the newest version (1:2.39.5-0+deb12u1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

2/
```
crea@tpos:~$ which firefox
/usr/bin//firefox
crea@tpos:~$ firefox
```

3/
```
crea@tpos:~$ cat /etc/apt/sources.list
#deb cdrom:[Debian GNU/Linux 12.7.0 _Bookworm_ - Official amd64 NETINST with firmware 20240831-10:38]/ bookworm contrib main non-free-firmware

deb http://deb.debian.org/debian/ bookworm main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware

deb http://security.debian.org/debian-security bookworm-security main non-free-firmware
deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware

# bookworm-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware

# This system was installed using small removable media
# (e.g. netinst, live or single CD). The matching "deb cdrom"
# entries were disabled at the end of the installation process.
# For information about how to configure apt package sources,
# see the sources.list(5) manual.
```

### IV. Poupée Russe

1/
```
wget https://gitlab.com/it4lik/b1-os/-/raw/main/tp/2/meow
--2024-11-10 17:40:49--  https://gitlab.com/it4lik/b1-os/-/raw/main/tp/2/meow
Resolving gitlab.com (gitlab.com)... 172.65.251.78, 2606:4700:90:0:f22e:fbec:5bed:a9b9
Connecting to gitlab.com (gitlab.com)|172.65.251.78|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 18016947 (17M) [application/octet-stream]
Saving to: ‘meow’

meow              100%[===================>]  17.18M  12.8MB/s    in 1.3s    

2024-11-10 17:40:51 (12.8 MB/s) - ‘meow’ saved [18016947/18016947]
```

2/
```
crea@tpos:~/Downloads$ file meow
meow: Zip archive data, at least v2.0 to extract, compression method=deflate
crea@tpos:~/Downloads$ mv meow meow.zip
crea@tpos:~/Downloads$ ls
meow.zip

```

3/
```
crea@tpos:~/Downloads$ unzip meow.zip
Archive:  meow.zip
  inflating: meow                    
crea@tpos:~/Downloads$ ls
meow  meow.zip
crea@tpos:~/Downloads$ file meow
meow: XZ compressed data, checksum CRC64
crea@tpos:~/Downloads$ mv meow meow.xz
crea@tpos:~/Downloads$ xz -d meow.xz
crea@tpos:~/Downloads$ file meow
meow: bzip2 compressed data, block size = 900k
crea@tpos:~/Downloads$ bzip2 -d meow
bzip2: Can't guess original name for meow -- using meow.out
file meow.out
meow.out: RAR archive data, v5
crea@tpos:~/Downloads$ sudo apt install unrar-free
crea@tpos:~/Downloads$ unrar meow.out

unrar-free 0.1.3  Copyright (C) 2004  Ben Asselstine, Jeroen Dekkers


Extracting from /home/crea/Downloads/meow.out

Extracting  meow                                                      OK        
All OK
crea@tpos:~/Downloads$ file meow
meow: gzip compressed data, from Unix, original size modulo 2^32 145049600 gzip compressed data, reserved method, has CRC, extra field, has comment, from FAT filesystem (MS-DOS, OS/2, NT), original size modulo 2^32 145049600
crea@tpos:~/Downloads$ mv meow.gzip meow.gz
crea@tpos:~/Downloads$ gzip -d meow.gz
crea@tpos:~/Downloads$ file meow
meow: POSIX tar archive (GNU)
crea@tpos:~/Downloads$ mv meow meow.tar
crea@tpos:~/Downloads$ tar -xvf meow.tar
```

4/
```
crea@tpos:~/Downloads$ find dawa/ -type f -size 15M
dawa/folder31/19/file39
-
crea@tpos:~/Downloads$ find dawa/ -type f -name "cookie"
dawa/folder14/25/cookie
-
crea@tpos:~/Downloads$ find dawa/ -type f -name ".*"
dawa/folder32/14/.hidden_file
-
crea@tpos:~/Downloads$ find dawa/ -type f -path "*/*/*/*/*/*"
dawa/folder37/45/23/43/54/file43
-
crea@tpos:~/Downloads$ find dawa/ -type f -newermt 2014-01-01 ! -newermt 2015-01-01
dawa/folder36/40/file43
-
crea@tpos:~/Downloads$ find dawa/ -type f | xargs grep -L '[^7]'
dawa/folder43/38/file41
```

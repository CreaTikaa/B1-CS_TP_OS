 1 : 

1/ 
```
ps -ef / Tout les process en cours avec leurs ID et noms
```
2/ 
```
ps -e --sort=pid | head -n 4 (1 = en tete) / PID 1 systemd PID 2 kthreadd PID 3 pool_workqueue_release
```
3/ 
```
systemctl list-units --type=service --state=running & systemctl list-units --type=service --all
 UNIT                           LOAD   ACTIVE SUB     DESCRIPTION             >
  accounts-daemon.service        loaded active running Accounts Service
  colord.service                 loaded active running Manage, Install and Gene>
  cron.service                   loaded active running Regular background progr>
  &
UNIT                                     LOAD      ACTIVE   SUB     DESCRIPTI>
  accounts-daemon.service                  loaded    active   running Accounts >
  apparmor.service                         loaded    inactive dead    Load AppA>
  apt-daily-upgrade.service                loaded    inactive dead    Daily apt>
```
  
###  2 | Mémoire & CPU : 
1/
```
free -h & grep MemTotal /proc/meminfo
               total        used        free      shared  buff/cache   available
Mem:           8.0Gi       2.4Gi       4.8Gi        65Mi       1.1Gi       5.6Gi & MemTotal:        8438468 kB
Swap:          974Mi          0B       974Mi
2/ top / 
16:34:12 up 55 min,  1 user,  load average: 1.08, 1.02, 0.80
Tasks: 206 total,   3 running, 203 sleeping,   0 stopped,   0 zombie
%Cpu(s): 17.5 us, 18.5 sy,  0.0 ni, 63.2 id,  0.1 wa,  0.0 hi,  0.6 si,  0.0 st
```
 ### 3 | Stockage : 
 
1/
```
lsblk -o NAME,SIZE,MODEL,TYPE | grep disk (lsblk -d -o NAME,SIZE,MODEL montre sr0  51M VBOX CD-ROM donc j'ai pas mis celle la en réponse)
sda      60G VBOX HARDDISK disk
2/ sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT ou simplement lsbk -a (donne un peu + de résultats)
NAME   FSTYPE   SIZE MOUNTP
sda              60G 
├─sda1 ext4      59G /
├─sda2            1K 
└─sda5 swap     975M [SWAP]
sr0    iso9660   51M 
3/ df -h /
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        58G   26G   30G  47% /
```

### 4 | Réseau : 
1/
```
ifconfig 
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::a00:27ff:fe8c:c906  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:8c:c9:06  txqueuelen 1000  (Ethernet)
        RX packets 64058  bytes 69891242 (66.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 22312  bytes 6097264 (5.8 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 34  bytes 2020 (1.9 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 2020 (1.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

Plus court : ip -brief addr ou lshw -class network -short (pas installé de base)
lo               UNKNOWN        127.0.0.1/8 ::1/128 
eth0             UP             10.0.2.15/24 fe80::a00:27ff:fe8c:c906/64
```

2/ 
```
sudo ss -t -a -p state established

Recv-Q     Send-Q         Local Address:Port             Peer Address:Port      Process                                                                         
0          0                  10.0.2.15:46614          172.64.155.209:https      users:(("firefox-esr",pid=5117,fd=32))                                         
0          0                  10.0.2.15:43324          34.202.201.250:https      users:(("firefox-esr",pid=5117,fd=81))                                         
0          0                  10.0.2.15:46246           34.107.243.93:https      users:(("firefox-esr",pid=5117,fd=82))                                         
0          0                  10.0.2.15:35904            104.18.32.47:https      users:(("firefox-esr",pid=5117,fd=78))                                         
0          0                  10.0.2.15:50020            34.120.22.49:https      users:(("firefox-esr",pid=5117,fd=80))                                         
0          0                  10.0.2.15:56182          216.58.214.162:https      users:(("firefox-esr",pid=5117,fd=223))                                        
0          0                  10.0.2.15:52506           172.65.251.78:https      users:(("firefox-esr",pid=5117,fd=79))
```

 ### 5 | Utilisateurs 
1/ 
```
cat /etc/passwd, whoami pour voir soi même, getent group sudo pour voir ceux qui peuvent use sudo result : sudo:x:27:crea (getent passwd 0 pr afficher root)
root:x:0:0:root:/root:/usr/bin/zsh
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
etc, etc
```
2/ 
```
w ou who -u                           
 17:04:11 up  1:25,  1 user,  load average: 2.50, 2.28, 1.80
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
crea     tty2     -                15:39    1:25m  9:35   0.04s /usr/libexec/gnome-session-binary
        ---                                                                                                                                                                                                                                    
crea     seat0        2024-11-04 15:39   ?          2186 (login screen)
crea     :1           2024-11-04 15:39   ?          2186 (:1)
```
3/
 ```
top ou ps -u $(whoami) pour les process lancés par soi même, changer whoami pr chercher par user
 PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                                                                                                
   2192 crea      20   0  455532 177540 114600 S  48.2   2.1  10:24.13 Xorg                                                                                                                                                                   
   5117 crea      20   0   11.9g 692240 285324 S  38.5   8.2   8:48.68 firefox-esr                                                                                                                                                            
   2390 crea      20   0 4501092 486948 170672 S  24.6   5.8  13:26.88 gnome-shell                                                                                                                                                            
   7844 crea      20   0 2966728 281324 109568 S  16.6   3.3   0:54.74 Isolated Web Co                                                                                                                                                        
   2445 crea      20   0 1357408 142016  78000 S   3.0   1.7   0:10.62 mutter-x11-fram                                                                                                                                                        
   6153 crea      20   0  811468  88088  50560 S   0.7   1.0   0:07.82 terminator
```  
4/ 
```
ls -l ch9.pcap
-rw-r--r-- 1 crea crea 5048 Sep 10 12:14 ch9.pcap
```

 ### 6 | Random 
 
1/
```
uptime -s
2024-11-04 15:38:38
```
2/ 
```
cat /proc/cpuinfo | grep "model name" juste pour le nom ou lscpu pour + d'infos
model name	: 11th Gen Intel(R) Core(TM) i7-11370H @ 3.30GHz
```
3/ 
```
hostnamectl
 Static hostname: crea
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 99269403856d4784b6e17ae276bfbd8a
         Boot ID: 7ab5fcc535464520a755d21ab3cff381
  Virtualization: oracle
Operating System: Kali GNU/Linux Rolling          
          Kernel: Linux 6.6.9-amd64
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
Firmware Version: VirtualBox
```
4/ 
```
ls -l /var/lib/dpkg/status
-rw-r--r-- 1 root root 2894509 Nov  4 16:50 /var/lib/dpkg/status
```

### 7 | Amusement : 
1/ 
```
netstat -tnp
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 10.0.2.15:59092         34.117.188.166:443      ESTABLISHED 5117/firefox-esr
```   
2/ 
```
s -p 5117 -o user,%mem
USER     %MEM
crea      8.9
```
3/ 
```
which firefox-esr ou whereis
/usr/bin/firefox-esr
ls -l /usr/bin/firefox-esr
lrwxrwxrwx 1 root root 30 Jan 23  2024 /usr/bin/firefox-esr -> ../lib/firefox-esr/firefox-esr
```
4/ 
```
curl http://ip-api.com/json/13.37.13.37
{"status":"success","country":"France","countryCode":"FR","region":"IDF","regionName":"Île-de-France","city":"Paris","zip":"75000","lat":48.8566,"lon":2.35222,"timezone":"Europe/Paris","isp":"Amazon Technologies Inc.","org":"AWS EC2 (eu-west-3)","as":"AS16509 Amazon.com, Inc.","query":"13.37.13.37"} 
ping 13.37.13.37
PING 13.37.13.37 (13.37.13.37) 56(84) bytes of data.
```







 


# TP 5  
## Partie I : Compilation étou tavu  
### 0. Prérequis  
1/ Créer un répertoire de travail dans votre répertoire personnel
```
crea@tpos:~$ pwd
/home/crea
crea@tpos:~$ mkdir work
crea@tpos:~$ cd work
crea@tpos:~/work$ pwd
/home/crea/work
crea@tpos:~/work$ 
```
### I. Programme minimal  
**2. Analyse du programme compilé**  
1/ Retrouvez à l'aide de readelf l'architecture pour laquelle le programme est compilé
```
crea@tpos:~/work$ readelf -h hello1
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Position-Independent Executable file)
  Machine:                           Intel 80386
  Version:                           0x1
  Entry point address:               0x1000
  Start of program headers:          52 (bytes into file)
  Start of section headers:          13304 (bytes into file)
  Flags:                             0x0
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         11
  Size of section headers:           40 (bytes)
  Number of section headers:         21
  Section header string table index: 20
```
2/ Afficher la liste des sections contenues dans le programme
```
crea@tpos:~/work$ readelf -S hello1
There are 21 section headers, starting at offset 0x33f8:

Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .interp           PROGBITS        00000194 000194 000013 00   A  0   0  1
  [ 2] .note.gnu.bu[...] NOTE            000001a8 0001a8 000024 00   A  0   0  4
  [ 3] .gnu.hash         GNU_HASH        000001cc 0001cc 000018 04   A  4   0  4
  [ 4] .dynsym           DYNSYM          000001e4 0001e4 000010 10   A  5   1  4
  [ 5] .dynstr           STRTAB          000001f4 0001f4 000001 00   A  0   0  1
  [ 6] .text             PROGBITS        00001000 001000 00005d 00  AX  0   0  1
  [ 7] .eh_frame_hdr     PROGBITS        00002000 002000 00001c 00   A  0   0  4
  [ 8] .eh_frame         PROGBITS        0000201c 00201c 000058 00   A  0   0  4
  [ 9] .dynamic          DYNAMIC         00003f8c 002f8c 000068 08  WA  5   0  4
  [10] .got.plt          PROGBITS        00003ff4 002ff4 00000c 04  WA  0   0  4
  [11] .comment          PROGBITS        00000000 003000 00001f 01  MS  0   0  1
  [12] .debug_aranges    PROGBITS        00000000 00301f 000020 00      0   0  1
  [13] .debug_info       PROGBITS        00000000 00303f 000070 00      0   0  1
  [14] .debug_abbrev     PROGBITS        00000000 0030af 000062 00      0   0  1
  [15] .debug_line       PROGBITS        00000000 003111 000054 00      0   0  1
  [16] .debug_str        PROGBITS        00000000 003165 000085 01  MS  0   0  1
  [17] .debug_line_str   PROGBITS        00000000 0031ea 000019 01  MS  0   0  1
  [18] .symtab           SYMTAB          00000000 003204 0000b0 10     19   6  4
  [19] .strtab           STRTAB          00000000 0032b4 00006a 00      0   0  1
  [20] .shstrtab         STRTAB          00000000 00331e 0000d9 00      0   0  1
```
3/ Affichez le code assembleur de la section .text à l'aide d'objdump
```
crea@tpos:~/work$ objdump -M intel -j .text -d hello1

hello1:     file format elf32-i386


Disassembly of section .text:

00001000 <_start>:
    1000:	55                   	push   ebp
    1001:	89 e5                	mov    ebp,esp
    1003:	57                   	push   edi
    1004:	56                   	push   esi
    1005:	53                   	push   ebx
    1006:	83 ec 10             	sub    esp,0x10
    1009:	e8 4b 00 00 00       	call   1059 <__x86.get_pc_thunk.ax>
    100e:	05 e6 2f 00 00       	add    eax,0x2fe6
    1013:	c7 45 e5 48 65 6c 6c 	mov    DWORD PTR [ebp-0x1b],0x6c6c6548
    101a:	c7 45 e9 6f 2c 20 57 	mov    DWORD PTR [ebp-0x17],0x57202c6f
    1021:	c7 45 ed 6f 72 6c 64 	mov    DWORD PTR [ebp-0x13],0x646c726f
    1028:	c7 45 f0 64 21 0a 00 	mov    DWORD PTR [ebp-0x10],0xa2164
    102f:	8d 75 e5             	lea    esi,[ebp-0x1b]
    1032:	bf 0e 00 00 00       	mov    edi,0xe
    1037:	b8 04 00 00 00       	mov    eax,0x4
    103c:	bb 01 00 00 00       	mov    ebx,0x1
    1041:	89 f1                	mov    ecx,esi
    1043:	89 fa                	mov    edx,edi
    1045:	cd 80                	int    0x80
    1047:	b8 01 00 00 00       	mov    eax,0x1
    104c:	31 db                	xor    ebx,ebx
    104e:	cd 80                	int    0x80
    1050:	90                   	nop
    1051:	83 c4 10             	add    esp,0x10
    1054:	5b                   	pop    ebx
    1055:	5e                   	pop    esi
    1056:	5f                   	pop    edi
    1057:	5d                   	pop    ebp
    1058:	c3                   	ret

00001059 <__x86.get_pc_thunk.ax>:
    1059:	8b 04 24             	mov    eax,DWORD PTR [esp]
    105c:	c3                   	ret
```
**2. Librairie et compilation dynamique**  
*C. Tracer les appels à des librairies*
1/ Tracez à l'aide de la commande ldd les librairies appelées par… hello2  puis hello1 puis ls
```
crea@tpos:~/work$ ldd hello2
	linux-gate.so.1 (0xf7f44000)
	libc.so.6 => /lib32/libc.so.6 (0xf7d00000)
	/lib/ld-linux.so.2 (0xf7f46000)
crea@tpos:~/work$ ldd hello1
	statically linked
crea@tpos:~/work$ which ls
/usr/bin/ls
crea@tpos:~/work$ ldd /usr/bin/ls
	linux-vdso.so.1 (0x00007ffe7f3fd000)
	libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007f915dedd000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f915dcfc000)
	libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007f915dc62000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f915df44000)
crea@tpos:~$ ldd /usr/bin/firefox
	not a dynamic executable
```
2/ Vérif type fichier
```
crea@tpos:~/work$ file /usr/bin/ls
/usr/bin/ls: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=15dfff3239aa7c3b16a71e6b2e3b6e4009dab998, for GNU/Linux 3.2.0, stripped
crea@tpos:~/work$ file /usr/bin/firefox
/usr/bin/firefox: POSIX shell script, ASCII text executable
```
3/ Parmi les librairies appelées par hello2, déterminer le type du fichier nommé libc.so.X
```
crea@tpos:~/work$ ldd hello2
	linux-gate.so.1 (0xf7f29000)
	libc.so.6 => /lib32/libc.so.6 (0xf7ce5000)
	/lib/ld-linux.so.2 (0xf7f2b000)
crea@tpos:~/work$ file /lib32/lib.so.6
/lib32/lib.so.6: cannot open `/lib32/lib.so.6' (No such file or directory)
crea@tpos:~/work$ sudo find / -name "libc.so.6"
/usr/powerpc-linux-gnu/lib/libc.so.6
/usr/powerpc64-linux-gnu/lib/libc.so.6
/usr/mipsel-linux-gnu/lib/libc.so.6
/usr/libx32/libc.so.6
/usr/lib32/libc.so.6
/usr/lib/x86_64-linux-gnu/libc.so.6
/usr/i686-linux-gnu/lib/libc.so.6
/usr/sparc64-linux-gnu/lib/libc.so.6
/usr/mips-linux-gnu/lib/libc.so.6
/usr/arm-linux-gnueabi/lib/libc.so.6
/usr/riscv64-linux-gnu/lib/libc.so.6
find: ‘/run/user/1000/doc’: Permission denied
crea@tpos:~/work$ file /usr/lib32/libc.so.6
/usr/lib32/libc.so.6: ELF 32-bit LSB shared object, Intel 80386, version 1 (GNU/Linux), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=b3f5646d25dc90cc34d2507f197561065c7e630c, for GNU/Linux 3.2.0, stripped
```
**3. Compilation statique**  
1/ Affichez le type des fichiers hello2 et hello3
```
crea@tpos:~/work$ cp hello2.c hello3.c
crea@tpos:~/work$ gcc -static -fno-stack-protector -g -m32 -o hello3 hello3.c
crea@tpos:~/work$ file hello2
hello2: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=1af634bb9fbac19071b8997e7182848074215d22, for GNU/Linux 3.2.0, with debug_info, not stripped
crea@tpos:~/work$ file hello3
hello3: ELF 32-bit LSB executable, Intel 80386, version 1 (GNU/Linux), statically linked, BuildID[sha1]=90c5517dcac9e2c47bf10ce0f6cb305ef54abc36, for GNU/Linux 3.2.0, with debug_info, not stripped
```
2/ Affichez leurs tailles
```
crea@tpos:~/work$ du -h hello2
16K	hello2
crea@tpos:~/work$ du -h hello3
728K	hello3
```
**4. Compilation cross-platform**  
1/ Affichez l'architecture de votre CPU
```
crea@tpos:~/work$ cat /proc/cpuinfo
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 140
model name	: 11th Gen Intel(R) Core(TM) i7-11370H @ 3.30GHz
stepping	: 1
microcode	: 0xffffffff
cpu MHz		: 3302.400
cache size	: 12288 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 22
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 movbe popcnt aes rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single ibrs_enhanced fsgsbase bmi1 bmi2 invpcid rdseed adx clflushopt sha_ni arat md_clear flush_l1d arch_capabilities
bugs		: spectre_v1 spectre_v2 spec_store_bypass swapgs retbleed eibrs_pbrsb bhi
bogomips	: 6604.80
clflush size	: 64
cache_alignment	: 64
address sizes	: 39 bits physical, 48 bits virtual
power management:
```
2/ Vérifiez que vous avez la commande x86-64-linux-gnu-gcc
```
crea@tpos:~$ sudo find / -name "x86_64-linux-gnu-gcc"
/usr/bin/x86_64-linux-gnu-gcc
```
3/ Compilez votre fichier hello3.c dans un fichier cible nommé hello4 vers une autre architecture que la vôtre
```
crea@tpos:~$ sudo apt install gcc-arm-linux-gnueabihf
crea@tpos:~/work$ touch hello4
crea@tpos:~/work$ arm-linux-gnueabihf-gcc hello3.c -o hello4
```
4/ Désassemblez hello3 et hello4 à l'aide d'objdump
```
objdump -M intel -j .text -d hello3
objdump -M intel -j .text -d hello4
```
5/ Essayez d'exécuter le programme hello4
```
crea@tpos:~/work$ ./hello4
bash: ./hello4: cannot execute binary file: Exec format error
```
## Partie II : Crack des trucz
**1. Install Ghidra**  
1/
```
crea@tpos:~/work$ apt search ghidra
Sorting... Done
Full Text Search... Done
```
2/
```
crea@tpos:~/work$ sudo nano /etc/apt/sources.list
```
3/
```
crea@tpos:~/work$ sudo wget https://archive.kali.org/archive-key.asc -O /etc/apt/trusted.gpg.d/kali-archive-keyring.asc
--2024-11-18 18:09:59--  https://archive.kali.org/archive-key.asc
Resolving archive.kali.org (archive.kali.org)... 192.99.45.140, 2607:5300:60:508c::
Connecting to archive.kali.org (archive.kali.org)|192.99.45.140|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3155 (3.1K) [application/octet-stream]
Saving to: ‘/etc/apt/trusted.gpg.d/kali-archive-keyring.asc’

/etc/apt/trusted.gp 100%[===================>]   3.08K  --.-KB/s    in 0s      

2024-11-18 18:10:00 (69.8 MB/s) - ‘/etc/apt/trusted.gpg.d/kali-archive-keyring.asc’ saved [3155/3155]
```
**2. Patch manuel programme simple**  
1/
Fait avec Ghidra, changement JNZ en JZ
```
crea@tpos:~$ ./password_3.exe mauvaismdp
Access granted! Spawning shell...
$  
```
**4. Ptit chall maison**  
1/ 
```
crea@tpos:~/work/ghidra_11.2.1_PUBLIC$ wget https://gitlab.com/it4lik/b1-os/-/raw/main/tp/5/kaddate_challenge
--2024-11-20 11:03:02--  https://gitlab.com/it4lik/b1-os/-/raw/main/tp/5/kaddate_challenge
Resolving gitlab.com (gitlab.com)... 172.65.251.78, 2606:4700:90:0:f22e:fbec:5bed:a9b9
Connecting to gitlab.com (gitlab.com)|172.65.251.78|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16208 (16K) [application/octet-stream]
Saving to: ‘kaddate_challenge.1’

kaddate_challenge.1 100%[===================>]  15.83K  --.-KB/s    in 0.02s   

2024-11-20 11:03:03 (706 KB/s) - ‘kaddate_challenge.1’ saved [16208/16208]
```
2/ readelf
```
crea@tpos:~/work/ghidra_11.2.1_PUBLIC$ readelf -h kaddate_challenge 
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Position-Independent Executable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x10c0
  Start of program headers:          64 (bytes into file)
  Start of section headers:          14288 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         13
  Size of section headers:           64 (bytes)
  Number of section headers:         30
  Section header string table index: 29
```
3/ Ghidra
```
crea@tpos:~$ ./kaddate_challenge3
Can you be the first to send us a Message ?
Enter you message: 
a
Congrats a, you are the first user (count=5).
Flag: b1-os{PetitB1deviendraGrandPetitB1}
```



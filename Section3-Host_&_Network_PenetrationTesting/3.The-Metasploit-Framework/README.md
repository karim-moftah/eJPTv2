# Section 3 course 3: Host & Network Penetration Testing: The Metasploit Framework (MSF)



### Table of Contents

- Metasploit Framework Overview
- Metasploit Fundamentals
  - Installing & Configuring The Metasploit Framework
  - MSFconsole Fundamentals    
  - Creating & Managing Workspaces    
- Nmap
  - import nmap scan to metasploit

- Enumeration
- Vulnerability Scanning









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
















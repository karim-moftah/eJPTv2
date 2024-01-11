# Section 1 course 1: Assessment Methodologies: Information Gathering

### Table of Contents

- [Passive information gathering](#1--passive-information-gathering)
- [Active Information Gathering](#2--active-information-gathering)



### Information Gathering

Information Gathering is the process of collecting, organizing and  analyzing data and intelligence about a target, such as computer  network, website or individual, to identify vulnerabilities that can be  used for research, threat analysis, strategic planning, or exploited for offensive and/or defensive operations.

### Passive information gathering

- identifying IP addresses & DNS information
- identifying domain names and domain ownership information
- identifying email addresses and social media profiles
- identifying web technologies being used on the target sites
- identifying subdomains
- identifying directories hidden from search engine

### Active information gathering

- discovering open ports on target systems
- learning about the internal infrastructure of a target network/organization 
- enumerating information from the target systems

---

### 1- Passive information gathering

#### 1- host

The **host** command on Linux systems can look up a variety of information available through the Domain Name System (DNS). It can  find a host name if given an IP address or an IP address if given a host name plus a lot of other interesting details on systems and internet  domains.

```bash
└─$  host hackersploit.org  
hackersploit.org has address 188.114.96.6
hackersploit.org has address 188.114.97.6
hackersploit.org has IPv6 address 2a06:98c1:3121::6
hackersploit.org has IPv6 address 2a06:98c1:3120::6
hackersploit.org mail is handled by 0 _dc-mx.2c2a3526b376.hackersploit.org.
```

there are two ip addresses (two IPv4, two IPv6) because the website is behind cloudflare firewall

<br />

#### 2- robots.txt

Robots.txt is a text file webmasters create to instruct web robots  (typically search engine robots) how to crawl pages on their website.

```bash
https://hackersploit.org/robots.txt  
```

<br />

#### 3- sitemap.xml

An XML sitemap is a file that lists a website’s essential pages, making  sure Google can find and crawl them all. It also helps search engines  understand your website structure. You want Google to crawl every  important page of your website. But sometimes, pages end up without  internal links pointing to them, making them hard to find. A sitemap can help speed up content discovery.

```bash
https://hackersploit.org/sitemap.xml
```

<br />

#### 4- add-on browser extentions

- wappalyzer
- builtwith

<br />

#### 5- whatweb

```bash
└─$ whatweb hackersploit.org  
http://hackersploit.org [301 Moved Permanently] Country[EUROPEAN UNION][EU], HTTPServer[cloudflare], IP[188.114.96.7], RedirectLocation[https://hackersploit.org/], UncommonHeaders[report-to,nel,cf-ray,alt-svc]
https://hackersploit.org/ [403 Forbidden] Country[EUROPEAN UNION][EU], HTML5, HTTPServer[cloudflare], IP[188.114.97.6], Title[403 Forbidden][Title element contains newline(s)!], UncommonHeaders[referrer-policy,x-turbo-charged-by,cf-cache-status,report-to,nel,cf-ray,alt-svc]
```

<br />

#### 6- HTTrack

website copier

<br />

#### 7- whois

```bash
└─$ whois hackersploit.org  
Domain Name: hackersploit.org
Registry Domain ID: 77f8fe62a425487cbefef4bf7e27d2ec-LROR
Registrar WHOIS Server: whois.namecheap.com
Registrar URL: http://www.namecheap.com
Updated Date: 2023-12-20T10:47:46Z
Creation Date: 2018-04-05T11:27:07Z
Registry Expiry Date: 2025-04-05T11:27:07Z
Registrar: NameCheap, Inc.
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.6613102107
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: REDACTED FOR PRIVACY

```

<br />

#### 8- [Netcraft](https://www.netcraft.com/tools/)

<br />

#### 9- dnsrecon

DNSRecon is a Python script that provides the ability to perform:

- Check all NS Records for Zone Transfers.
- Enumerate General DNS Records for a given Domain (MX, SOA, NS, A, AAAA, SPF and TXT).
- Perform common SRV Record Enumeration.
- Top Level Domain (TLD) Expansion.
- Check for Wildcard Resolution.
- Brute Force subdomain and host A and AAAA records given a domain and a wordlist.
- Perform a PTR Record lookup for a given IP Range or CIDR.
- Check a DNS Server Cached records for A, AAAA and CNAME
- Records provided a list of host records in a text file to check.
- Enumerate Hosts and Subdomains using Google

```bash
└─$ dnsrecon -d hackersploit.org     

[*] std: Performing General Enumeration against: hackersploit.org...
[*] DNSSEC is configured for hackersploit.org
[*] DNSKEYs:
[*]     NSEC ZSK ECDSAP256SHA256 a09311112cf9138818cd2feae970ebbd 4d6a30f6088c25b325a39abbc5cd1197 aa098283e5aaf421177c2aa5d714992a 9957d1bcc18f98cd71f1f1806b65e148
[*]     NSEC KSk ECDSAP256SHA256 99db2cc14cabdc33d6d77da63a2f15f7 1112584f234e8d1dc428e39e8a4a97e1 aa271a555dc90701e17e2a4c4b6f120b 7c32d44f4ac02bd894cf2d4be7778a19
[*]      SOA dee.ns.cloudflare.com 108.162.192.93
[*]      SOA dee.ns.cloudflare.com 172.64.32.93
[*]      SOA dee.ns.cloudflare.com 173.245.58.93
[*]      SOA dee.ns.cloudflare.com 2a06:98c1:50::ac40:205d
[*]      SOA dee.ns.cloudflare.com 2606:4700:50::adf5:3a5d
[*]      SOA dee.ns.cloudflare.com 2803:f800:50::6ca2:c05d
[*]      NS dee.ns.cloudflare.com 108.162.192.93
[-]      Recursion enabled on NS Server 108.162.192.93
[*]      NS dee.ns.cloudflare.com 172.64.32.93
[-]      Recursion enabled on NS Server 172.64.32.93
[*]      NS dee.ns.cloudflare.com 173.245.58.93
[-]      Recursion enabled on NS Server 173.245.58.93
[*]      NS dee.ns.cloudflare.com 2606:4700:50::adf5:3a5d
[*]      NS dee.ns.cloudflare.com 2803:f800:50::6ca2:c05d
[*]      NS dee.ns.cloudflare.com 2a06:98c1:50::ac40:205d
[*]      NS jim.ns.cloudflare.com 108.162.193.125
[-]      Recursion enabled on NS Server 108.162.193.125
[*]      NS jim.ns.cloudflare.com 172.64.33.125
[-]      Recursion enabled on NS Server 172.64.33.125
[*]      NS jim.ns.cloudflare.com 173.245.59.125
[-]      Recursion enabled on NS Server 173.245.59.125
[*]      NS jim.ns.cloudflare.com 2803:f800:50::6ca2:c17d
[*]      NS jim.ns.cloudflare.com 2a06:98c1:50::ac40:217d
[*]      NS jim.ns.cloudflare.com 2606:4700:58::adf5:3b7d
[*]      MX _dc-mx.2c2a3526b376.hackersploit.org 198.54.120.212
[*]      A hackersploit.org 188.114.97.7
[*]      A hackersploit.org 188.114.96.7
[*]      AAAA hackersploit.org 2a06:98c1:3120::7
[*]      AAAA hackersploit.org 2a06:98c1:3121::7
[*]      TXT hackersploit.org google-site-verification=TW0pQsFZ0xx3w4b7kysBV0UrcMq7fJFB-5Rz9h6GwkU
[*]      TXT hackersploit.org v=spf1 +ip4:198.54.120.203 +include:spf.web-hosting.com +ip4:198.54.120.212 +include:hackersploit.org ~all
[*] Enumerating SRV Records
[+] 0 Records Found

```

**Note:** [*]      MX _dc-mx.2c2a3526b376.hackersploit.org 198.54.120.212

cloudflare does not proxy mail server address  

<br />

#### 10- [dnsdumpster](https://dnsdumpster.com/)

<br />

#### 11- wafw00f

Web Application Firewall Fingerprinting Toolkit and this information we can not know it from whois lookup or dns lookup

to view all firewalls that wafw00f can detect 

                      ```
                      └─$ wafw00f -l
                      ```

<br />

```bash
└─$ wafw00f hackersploit.org
                             
[*] Checking https://hackersploit.org
[+] The site https://hackersploit.org is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2

```

<br />

to make it verbose

```bash
└─$ wafw00f -vv hackersploit.org 

INFO:wafw00f:The url hackersploit.org should start with http:// or https:// .. fixing (might make this unusable)
[*] Checking https://hackersploit.org
INFO:wafw00f:starting wafw00f on https://hackersploit.org
INFO:wafw00f:Request Succeeded
INFO:wafw00f:Request Succeeded
INFO:wafw00f:Checking for ACE XML Gateway (Cisco)
INFO:wafw00f:Checking for aeSecure (aeSecure)
INFO:wafw00f:Checking for AireeCDN (Airee)
INFO:wafw00f:Checking for Cloudbric (Penta Security)
INFO:wafw00f:Checking for Cloudflare (Cloudflare Inc.)
INFO:wafw00f:Identified WAF: ['Cloudflare (Cloudflare Inc.)']
[+] The site https://hackersploit.org is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2
INFO:wafw00f:Found: 1 matches.
                                  
```

<br />

#### 12- Sublist3r

Sublist3r is a python tool designed to enumerate subdomains of websites using OSINT. It helps penetration testers and bug hunters collect and gather subdomains for the domain they are targeting. Sublist3r enumerates subdomains using many search engines such as Google, Yahoo, Bing, Baidu and Ask. Sublist3r also enumerates subdomains using Netcraft, Virustotal, ThreatCrowd, DNSdumpster and ReverseDNS.

```bash
└─$ sublist3r -d ine.com
```

<br />

#### 13- Google dorks

- site:ine.com
- site:ine.com inurl:admin
- site:*.ine.com   => to get subdomains
- site:*.ine.com  intitle:admin 
- site:*.ine.com  filetype:pdf
- intitle:index of
- cache:ine.com  
- inurl:auth_user_file.txt 

<br />

#### 14- [Google hacking database](https://www.exploit-db.com/google-hacking-database)

<br />

#### 15- [archive.org](https://web.archive.org/)

<br />

#### 16- [have i been pwned?](https://haveibeenpwned.com/)

<br />

---



### 2- Active Information Gathering

#### 1- DNS Zone Transfers  using dig

```bash
└─# dig axfr @nsztm1.digi.ninja zonetransfer.me

; <<>> DiG 9.18.16-1-Debian <<>> axfr @nsztm1.digi.ninja zonetransfer.me
; (1 server found)
;; global options: +cmd
zonetransfer.me.        7200    IN      SOA     nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.        300     IN      HINFO   "Casio fx-700G" "Windows XP"
zonetransfer.me.        301     IN      TXT     "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.        7200    IN      MX      0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      10 ALT2.ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      A       5.196.105.14
zonetransfer.me.        7200    IN      NS      nsztm1.digi.ninja.
zonetransfer.me.        7200    IN      NS      nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me. 301 IN TXT     "6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN     SRV     0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200 IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN   AFSDB   1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200  IN      A       127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN    AFSDB   1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A      202.14.81.230
cmdexec.zonetransfer.me. 300    IN      TXT     "; ls"
contact.zonetransfer.me. 2592000 IN     TXT     "Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
dc-office.zonetransfer.me. 7200 IN      A       143.228.181.132
deadbeef.zonetransfer.me. 7201  IN      AAAA    dead:beaf::
dr.zonetransfer.me.     300     IN      LOC     53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
DZC.zonetransfer.me.    7200    IN      TXT     "AbCdEfG"
email.zonetransfer.me.  2222    IN      NAPTR   1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
email.zonetransfer.me.  7200    IN      A       74.125.206.26
Hello.zonetransfer.me.  7200    IN      TXT     "Hi to Josh and all his class"
home.zonetransfer.me.   7200    IN      A       127.0.0.1
Info.zonetransfer.me.   7200    IN      TXT     "ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
internal.zonetransfer.me. 300   IN      NS      intns1.zonetransfer.me.
internal.zonetransfer.me. 300   IN      NS      intns2.zonetransfer.me.
intns1.zonetransfer.me. 300     IN      A       81.4.108.41
intns2.zonetransfer.me. 300     IN      A       167.88.42.94
office.zonetransfer.me. 7200    IN      A       4.23.39.254
ipv6actnow.org.zonetransfer.me. 7200 IN AAAA    2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.    7200    IN      A       207.46.197.32
robinwood.zonetransfer.me. 302  IN      TXT     "Robin Wood"
rp.zonetransfer.me.     321     IN      RP      robin.zonetransfer.me. robinwood.zonetransfer.me.
sip.zonetransfer.me.    3333    IN      NAPTR   2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
sqli.zonetransfer.me.   300     IN      TXT     "' or 1=1 --"
sshock.zonetransfer.me. 7200    IN      TXT     "() { :]}; echo ShellShocked"
staging.zonetransfer.me. 7200   IN      CNAME   www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A 127.0.0.1
testing.zonetransfer.me. 301    IN      CNAME   www.zonetransfer.me.
vpn.zonetransfer.me.    4000    IN      A       174.36.59.154
www.zonetransfer.me.    7200    IN      A       5.196.105.14
xss.zonetransfer.me.    300     IN      TXT     "'><script>alert('Boo')</script>"
zonetransfer.me.        7200    IN      SOA     nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
;; Query time: 96 msec
;; SERVER: 81.4.108.41#53(nsztm1.digi.ninja) (TCP)
;; WHEN: Fri Dec 29 11:49:36 EST 2023
;; XFR size: 50 records (messages 1, bytes 2085)


```

<br />

#### 2- dnsenum

```bash
└─# dnsenum zonetransfer.me
dnsenum VERSION:1.2.6

-----   zonetransfer.me   -----                                              
                                                                             
                                                                             
Host's addresses:                                                            
__________________                                                           
                                                                             
zonetransfer.me.                         5        IN    A        5.196.105.14

                                                                             
Name Servers:                                                                
______________                                                               
                                                                             
nsztm1.digi.ninja.                       5        IN    A        81.4.108.41 
nsztm2.digi.ninja.                       5        IN    A        34.225.33.2

                                                                             
Mail (MX) Servers:                                                           
___________________                                                          
                                                                             
ASPMX4.GOOGLEMAIL.COM.                   5        IN    A        142.250.150.27
ALT1.ASPMX.L.GOOGLE.COM.                 5        IN    A        142.250.153.27
ASPMX3.GOOGLEMAIL.COM.                   5        IN    A        142.251.9.26
ASPMX5.GOOGLEMAIL.COM.                   5        IN    A        74.125.200.27
ASPMX.L.GOOGLE.COM.                      5        IN    A        173.194.76.26
ASPMX2.GOOGLEMAIL.COM.                   5        IN    A        142.250.153.27
ALT2.ASPMX.L.GOOGLE.COM.                 5        IN    A        142.251.9.27

                                                                             
Trying Zone Transfers and getting Bind Versions:                             
_________________________________________________                            
                                                                             
                                                                             
Trying Zone Transfer for zonetransfer.me on nsztm1.digi.ninja ... 
zonetransfer.me.                         7200     IN    SOA               (
zonetransfer.me.                         300      IN    HINFO        "Casio
zonetransfer.me.                         301      IN    TXT               (
zonetransfer.me.                         7200     IN    MX                0
zonetransfer.me.                         7200     IN    MX               10
zonetransfer.me.                         7200     IN    MX               10
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    A        5.196.105.14
zonetransfer.me.                         7200     IN    NS       nsztm1.digi.ninja.
zonetransfer.me.                         7200     IN    NS       nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me.         301      IN    TXT               (
_sip._tcp.zonetransfer.me.               14000    IN    SRV               0
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200     IN    PTR      www.zonetransfer.me.
asfdbauthdns.zonetransfer.me.            7900     IN    AFSDB             1
asfdbbox.zonetransfer.me.                7200     IN    A         127.0.0.1
asfdbvolume.zonetransfer.me.             7800     IN    AFSDB             1
canberra-office.zonetransfer.me.         7200     IN    A        202.14.81.230
cmdexec.zonetransfer.me.                 300      IN    TXT              ";
contact.zonetransfer.me.                 2592000  IN    TXT               (
dc-office.zonetransfer.me.               7200     IN    A        143.228.181.132
deadbeef.zonetransfer.me.                7201     IN    AAAA     dead:beaf::
dr.zonetransfer.me.                      300      IN    LOC              53
DZC.zonetransfer.me.                     7200     IN    TXT         AbCdEfG
email.zonetransfer.me.                   2222     IN    NAPTR             (
email.zonetransfer.me.                   7200     IN    A        74.125.206.26
Hello.zonetransfer.me.                   7200     IN    TXT             "Hi
home.zonetransfer.me.                    7200     IN    A         127.0.0.1
Info.zonetransfer.me.                    7200     IN    TXT               (
internal.zonetransfer.me.                300      IN    NS       intns1.zonetransfer.me.
internal.zonetransfer.me.                300      IN    NS       intns2.zonetransfer.me.
intns1.zonetransfer.me.                  300      IN    A        81.4.108.41
intns2.zonetransfer.me.                  300      IN    A        167.88.42.94
office.zonetransfer.me.                  7200     IN    A        4.23.39.254
ipv6actnow.org.zonetransfer.me.          7200     IN    AAAA     2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.                     7200     IN    A        207.46.197.32
robinwood.zonetransfer.me.               302      IN    TXT          "Robin
rp.zonetransfer.me.                      321      IN    RP                (
sip.zonetransfer.me.                     3333     IN    NAPTR             (
sqli.zonetransfer.me.                    300      IN    TXT              "'
sshock.zonetransfer.me.                  7200     IN    TXT             "()
staging.zonetransfer.me.                 7200     IN    CNAME    www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301      IN    A         127.0.0.1
testing.zonetransfer.me.                 301      IN    CNAME    www.zonetransfer.me.
vpn.zonetransfer.me.                     4000     IN    A        174.36.59.154
www.zonetransfer.me.                     7200     IN    A        5.196.105.14
xss.zonetransfer.me.                     300      IN    TXT      "'><script>alert('Boo')</script>"

Trying Zone Transfer for zonetransfer.me on nsztm2.digi.ninja ... 
 	the same data as nsztm1.digi.ninja
 	
Brute forcing with /usr/share/dnsenum/dns.txt:                               
_______________________________________________                              
                                                                             
                                                                             
                                                                             
zonetransfer.me class C netranges:                                           
___________________________________                                          
                                                                             
 4.23.39.0/24                                                                
 5.196.105.0/24
 52.91.28.0/24
 74.125.206.0/24
 81.4.108.0/24
 143.228.181.0/24
 167.88.42.0/24
 174.36.59.0/24
 202.14.81.0/24
 207.46.197.0/24

                                                                             
Performing reverse lookup on 2560 ip addresses:                              
________________________________________________                             
                                                                             
                                                                     
0 results out of 2560 IP addresses.

                                                                             
zonetransfer.me ip blocks:                                                   
___________________________                                                  
                                                                             
                                                                             
done.

```



<br />

#### 3- fierce

```bash
└─# fierce --domain zonetransfer.me 
NS: nsztm1.digi.ninja. nsztm2.digi.ninja.
SOA: nsztm1.digi.ninja. (81.4.108.41)
Zone: success
{<DNS name @>: '@ 7200 IN SOA nsztm1.digi.ninja. robin.digi.ninja. 2019100801 '
               '172800 900 1209600 3600\n'
               '@ 300 IN HINFO "Casio fx-700G" "Windows XP"\n'
               '@ 301 IN TXT '
               '"google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"\n'
               '@ 7200 IN MX 0 ASPMX.L.GOOGLE.COM.\n'
               '@ 7200 IN MX 10 ALT1.ASPMX.L.GOOGLE.COM.\n'
               '@ 7200 IN MX 10 ALT2.ASPMX.L.GOOGLE.COM.\n'
               '@ 7200 IN MX 20 ASPMX2.GOOGLEMAIL.COM.\n'
               '@ 7200 IN MX 20 ASPMX3.GOOGLEMAIL.COM.\n'
               '@ 7200 IN MX 20 ASPMX4.GOOGLEMAIL.COM.\n'
               '@ 7200 IN MX 20 ASPMX5.GOOGLEMAIL.COM.\n'
               '@ 7200 IN A 5.196.105.14\n'
               '@ 7200 IN NS nsztm1.digi.ninja.\n'
               '@ 7200 IN NS nsztm2.digi.ninja.',
 <DNS name _acme-challenge>: '_acme-challenge 301 IN TXT '
                             '"6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"',
 <DNS name _sip._tcp>: '_sip._tcp 14000 IN SRV 0 0 5060 www',
 <DNS name 14.105.196.5.IN-ADDR.ARPA>: '14.105.196.5.IN-ADDR.ARPA 7200 IN PTR '
                                       'www',
 <DNS name asfdbauthdns>: 'asfdbauthdns 7900 IN AFSDB 1 asfdbbox',
 <DNS name asfdbbox>: 'asfdbbox 7200 IN A 127.0.0.1',
 <DNS name asfdbvolume>: 'asfdbvolume 7800 IN AFSDB 1 asfdbbox',
 <DNS name canberra-office>: 'canberra-office 7200 IN A 202.14.81.230',
 <DNS name cmdexec>: 'cmdexec 300 IN TXT "; ls"',
 <DNS name contact>: 'contact 2592000 IN TXT "Remember to call or email Pippa '
                     'on +44 123 4567890 or pippa@zonetransfer.me when making '
                     'DNS changes"',
 <DNS name dc-office>: 'dc-office 7200 IN A 143.228.181.132',
 <DNS name deadbeef>: 'deadbeef 7201 IN AAAA dead:beaf::',
 <DNS name dr>: 'dr 300 IN LOC 53 20 56.558 N 1 38 33.526 W 0.00m',
 <DNS name DZC>: 'DZC 7200 IN TXT "AbCdEfG"',
 <DNS name email>: 'email 2222 IN NAPTR 1 1 "P" "E2U+email" "" '
                   'email.zonetransfer.me\n'
                   'email 7200 IN A 74.125.206.26',
 <DNS name Hello>: 'Hello 7200 IN TXT "Hi to Josh and all his class"',
 <DNS name home>: 'home 7200 IN A 127.0.0.1',
 <DNS name Info>: 'Info 7200 IN TXT "ZoneTransfer.me service provided by Robin '
                  'Wood - robin@digi.ninja. See '
                  'http://digi.ninja/projects/zonetransferme.php for more '
                  'information."',
 <DNS name internal>: 'internal 300 IN NS intns1\ninternal 300 IN NS intns2',
 <DNS name intns1>: 'intns1 300 IN A 81.4.108.41',
 <DNS name intns2>: 'intns2 300 IN A 167.88.42.94',
 <DNS name office>: 'office 7200 IN A 4.23.39.254',
 <DNS name ipv6actnow.org>: 'ipv6actnow.org 7200 IN AAAA '
                            '2001:67c:2e8:11::c100:1332',
 <DNS name owa>: 'owa 7200 IN A 207.46.197.32',
 <DNS name robinwood>: 'robinwood 302 IN TXT "Robin Wood"',
 <DNS name rp>: 'rp 321 IN RP robin robinwood',
 <DNS name sip>: 'sip 3333 IN NAPTR 2 3 "P" "E2U+sip" '
                 '"!^.*$!sip:customer-service@zonetransfer.me!" .',
 <DNS name sqli>: 'sqli 300 IN TXT "\' or 1=1 --"',
 <DNS name sshock>: 'sshock 7200 IN TXT "() { :]}; echo ShellShocked"',
 <DNS name staging>: 'staging 7200 IN CNAME www.sydneyoperahouse.com.',
 <DNS name alltcpportsopen.firewall.test>: 'alltcpportsopen.firewall.test 301 '
                                           'IN A 127.0.0.1',
 <DNS name testing>: 'testing 301 IN CNAME www',
 <DNS name vpn>: 'vpn 4000 IN A 174.36.59.154',
 <DNS name www>: 'www 7200 IN A 5.196.105.14',
 <DNS name xss>: 'xss 300 IN TXT "\'><script>alert(\'Boo\')</script>"'}

```

<br />

```bash
└─# fierce --domain hackersploit.org
NS: dee.ns.cloudflare.com. jim.ns.cloudflare.com.
SOA: dee.ns.cloudflare.com. (173.245.58.93)
Zone: failure
Wildcard: failure
  Found: forum.hackersploit.org. (172.67.202.99)
Found: mail.hackersploit.org. (104.21.44.180)

```

<br />

#### 4- Host discovery

```bash
└─# nmap -sn 192.168.6.0/24 
OR
└─# netdiscover                                   
```

<br />

#### 5- Port scanning

```bash
└─# nmap 192.168.6.128 
```

<br />

if the ping probs are blocked by the target use -Pn to not check if the host is online, just perform the port scan 

```bash
└─# nmap -Pn 192.168.6.128
```

<br />

scan specific ports

```bash
└─# nmap -Pn 192.168.6.128 -p 80,22
```

<br />

scan all ports 

```bash
└─# nmap -Pn 192.168.6.128 -p-
```

<br />

perform fast scan (most common 100 ports)

```bash
└─# nmap -Pn -F 192.168.6.128 
```

<br />

by default nmap performs a  TCP port scan, to perform UDP port scan use -sU

```bash
└─# nmap -Pn -sU 192.168.6.128 
```

<br />

increase the verbosity of the scan 

```bash
└─# nmap -Pn 192.168.6.128 -v
```

<br />

perform service version detection

```bash
└─# nmap -Pn -sV 192.168.6.128 
```

<br />

Detect the operating system on the victim

```bash
└─# nmap -Pn -O 192.168.6.128 
```

<br />

run scripts on the open ports 

```bash
└─# nmap -Pn -sV -sC 192.168.6.128 
```

<br />

perform aggressive scan ( combine -sV, -O, -sC)

```bash
└─# nmap -Pn -A 192.168.6.128 
```

<br />

speed up the scan with -T from T0 (slow) to T4 (insane)

```bash
└─# nmap -Pn -T4 192.168.6.128 
```

<br />

output the scan result into file

```bash
└─# nmap -Pn  192.168.6.128 -oN test.txt

└─# nmap -Pn  192.168.6.128 -oX test.xml
```


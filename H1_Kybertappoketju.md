# H1 Kybertappoketju
## x) Lue/katso/kuuntele ja tiivistä.

## a) Asenna Kali virtuaalikoneeseen.
Hain kuvan Kalille kali.org sivulta. https://www.kali.org/get-kali/#kali-installer-images
https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/

## b) Irrota Kali-virtuaalikone verkosta
Irrotin koneen netistä oikealta yläkulmasta ja testasin komennot ```ping 1.1.1.1``` ja ```ping 8.8.8.8```
![Näyttökuva 2025-04-01 153422](https://github.com/user-attachments/assets/3e699d0a-5e00-47c6-9e73-629104c156f0)
Virtuaalikone ei ollut netissä

https://www.youtube.com/watch?v=_aVzQ_qEfhc

## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -T4 -A localhost). Selitä komennon paramterit. Analysoi ja selitä tulokset. 
Komennolla ```nmap -T4 -A localhost```sain porttiskannattua virtuaalikoneen.

![Näyttökuva 2025-04-01 155713](https://github.com/user-attachments/assets/965cf977-c9b9-499f-96a1-3aac089e020f)

Koska virtuaalikone ei ollut netissä, 1000 porttia oli closed tilassa. DNS-palvelimia ei myöskään löytynyt.

## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.
Edellisen Linux-palvelimet kurssin ansiosta (https://github.com/MikoLiukk/Linux-Palvelimet-2025) minulle kaksi tutuinta demonia on Apache2 ja Openssh.

Asensin demonir roottina komennoilla ```sudo apt-get install -y apache2``` ja ```sudo apt-get install -y openssh-server```.
Demonit käynnistin komennoilla ```sudo systemctl start apache2``` ja ```sudo systemctl start ssh```.
Demonien tilan tarkistuksen sai komennoilla ```systemctl status apache2``` ja ```systemctl status ssh```.

![Näyttökuva 2025-04-01 161102](https://github.com/user-attachments/assets/1c8736b6-6c06-41ff-bb40-88d5551f2a08)

![Näyttökuva 2025-04-01 161151](https://github.com/user-attachments/assets/d5dbd001-be9b-4873-bbbb-d002a9752de1)

Lopuksi irrotin kalin netistä ja porttiskannasin uudelleen.

![Näyttökuva 2025-04-01 161330](https://github.com/user-attachments/assets/c4f49897-308f-4959-b8e8-ffadf8cf10b5)

Suurin ero oli SSH-hostkeys (22/tcp) ja Apache2 (80/tcp).

## e) Asenna Metasploitable 2 virtuaalikoneeseen
Latasin Metasploitable 2 verkosta osoitteesta https://sourceforge.net/projects/metasploitable/files/Metasploitable2/metasploitable-linux-2.0.0.zip/download. Tämän jälkeen zip-tiedosto piti purkaa omaan kansioonsa.
VirtualBoxissa loin uuden virtuaalitietokoneen, ainoat huomioitavat erot esimerkiksi Ubuntun asentamisen kanssa oli, että pitää valita subtype: other linux, version: other linux (64-bit) ja hard drive:ssa pitää valita jo valmis kovalely, mikä on aikaisemmin purettu metasploitable.vmdk.

## f) Host-only virtuaaliverkko VirtualBoxissa
Menin virtualboxin ylävalikosta File -> Tools -> Network Manager
Jouduin luomaan uuden host-only adapterin. 

![Näyttökuva 2025-04-01 162607](https://github.com/user-attachments/assets/8016652c-a9e2-4e95-beb5-2c23e092280b)

Uusi adapteri tämän jälkeen lisättiin kalille ja metasploitille. 

![Näyttökuva 2025-04-01 163010](https://github.com/user-attachments/assets/0b620f7d-27e7-4ff6-ae30-a7848402c40a)

Kun yritin käynnistää virtuaalitietokoneet uudelleen tuli minulle vastaan Linux-kurssilta tuttu virhekoodi E_FAIL (0x80004005)
Sain korjattua tämän sammuttamalla oman tietokoneeni hypervisorlaunchin ja käynnistämällä tietokoneeni uudelleen.

![Näyttökuva 2025-04-01 163555](https://github.com/user-attachments/assets/a511ee0f-aa0b-4dfd-b9c5-4e17e88995c2)

Tämän jälkeen virtuaalikoneiden käynnistys onnistui.

Käynnistin ensimmäisenä metasploitablen ja kirjauduin sisään (msfadmin:msfadmin). 
Tarkistin, että metasploitable ei ole verkossa komennolla ```ping 1.1.1.1```, tämän jälkeen komennolla ```ifconfig``` löysin vielä 192.168.67.4 ip-osoitteen.

![Näyttökuva 2025-04-01 200839](https://github.com/user-attachments/assets/500b6cdb-adb0-41fa-8228-3da7ba43cf4c)

Tämän jälkeen avasin kalin ja tarkisin, että se ei ole verkossa ```ping 1.1.1.1```.
```nmap -sn 192.168.67.4``` löytyi halutusti metasploitable.

![Näyttökuva 2025-04-01 200758](https://github.com/user-attachments/assets/c05043e3-3fde-4765-aea1-a54708f838b7)

Varmistin tämän curlaamalla osoitteen ```curl 192.168.67.4```. 

![Näyttökuva 2025-04-01 200820](https://github.com/user-attachments/assets/19f7bc43-1c7a-4582-9177-03a43a791a81)


## h) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -T4 -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta.
Komennolla ```nmap -A -T4 -p- 192.168.67.4``` sain tehtyä porttiskannauksen.
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-01 20:09 EEST
Nmap scan report for 192.168.67.4
Host is up (0.0088s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.3.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.67.3
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
23/tcp    open  telnet      Linux telnetd
25/tcp    open  smtp        Postfix smtpd
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|_    SSL2_RC4_128_WITH_MD5
|_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
|_ssl-date: 2025-04-01T17:13:24+00:00; -1s from scanner time.
53/tcp    open  domain      ISC BIND 9.4.2
| dns-nsid: 
|_  bind.version: 9.4.2
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
|_http-title: Metasploitable2 - Linux
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
111/tcp   open  rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      38708/tcp   mountd
|   100005  1,2,3      58349/udp   mountd
|   100021  1,3,4      46405/udp   nlockmgr
|   100021  1,3,4      60589/tcp   nlockmgr
|   100024  1          51345/udp   status
|_  100024  1          53485/tcp   status
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
512/tcp   open  exec        netkit-rsh rexecd
513/tcp   open  login
514/tcp   open  shell       Netkit rshd
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
| mysql-info: 
|   Protocol: 10
|   Version: 5.0.51a-3ubuntu5
|   Thread ID: 8
|   Capabilities flags: 43564
|   Some Capabilities: SwitchToSSLAfterHandshake, SupportsTransactions, Support41Auth, SupportsCompression, LongColumnFlag, ConnectWithDatabase, Speaks41ProtocolNew
|   Status: Autocommit
|_  Salt: |;NYTvxubkt$QOu"FxKN
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
|_ssl-date: 2025-04-01T17:13:24+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
| Not valid before: 2010-03-17T14:07:45
|_Not valid after:  2010-04-16T14:07:45
5900/tcp  open  vnc         VNC (protocol 3.3)
| vnc-info: 
|   Protocol version: 3.3
|   Security types: 
|_    VNC Authentication (2)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
|_http-title: Apache Tomcat/5.5
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
38708/tcp open  mountd      1-3 (RPC #100005)
53485/tcp open  status      1 (RPC #100024)
54833/tcp open  java-rmi    GNU Classpath grmiregistry
60589/tcp open  nlockmgr    1-4 (RPC #100021)
MAC Address: 08:00:27:35:AA:D4 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: METASPLOITABLE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
|_clock-skew: mean: 59m59s, deviation: 2h00m00s, median: -1s
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   NetBIOS computer name: 
|   Domain name: localdomain
|   FQDN: metasploitable.localdomain
|_  System time: 2025-04-01T13:13:13-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

TRACEROUTE
HOP RTT     ADDRESS
1   8.77 ms 192.168.67.4

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 242.57 seconds
```
Todella hyvien tapojen mukaan, laitoin skannauksen tulokset ChatGPT:hen promptina, koska maalikolla oli liikaa tietoa edessää.
Vastaukseksi sain, että portit 21/tcp, 1524/tcp ja 3306/tcp ovat kiinnostavimpia portteja.

#### 21/tcp 
- Käytössä on vsftpd 2.3.4, joka on tunnettu takaovesta, hyökkääjä voi päästä tämän avulla portille 6200 ja saada root-käyttäjän.
- Nopeasti googlen avulla löytyi heti muutama Backdoor Command Execution.

#### 1524/tcp
- Mahdollinen pääsy suoraan root-shelliin.
- Tahallisesti jätetty takaovi
- Netcatilla voi saada root-käyttöoikeudet heti

#### 3306/tcp
- Vanha MySQL-versio
- Todennäköisesti haavoittuvainen vanhuudensa takia.

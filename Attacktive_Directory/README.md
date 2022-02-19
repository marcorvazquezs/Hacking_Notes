# Attacktive Directory Room

- Performed nmap scan 

    `nmap -p- -A -nP 10.10.27.113`

    ![Nmap Results](2022-02-19-09-43-00.png)

- Found the DNS domain name `spookysec.local` 
- Used enum4linux to enumerate shares
  
  `enum4linux -a spookysec.local`

- Used `kerbrute` to check for valid usernames using `userlist.txt` 
  
  ```bash
  echo 10.10.27.113 spookysec.local > /etc/hosts
  kerbrute userenum -d spookysec.local --dc spookysec.local userlist.txt -t 100
  ```

  ![Kerbrute Results](2022-02-19-09-41-36.png)


## Abusing Kerberos
- Using the usernames we got from Kerbrute we are going to use Impacket's tool "GetNPUsers.py" to perform an attack method called ASREPRoasting   
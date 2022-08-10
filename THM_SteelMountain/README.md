# Steel Mountain
---

## Initial Access

- Scan the machine with `nmap`

![Nmap Results](screenshots/2022-08-03-18-45-33.png)

- Found [CVE-2014-6287](https://www.cvedetails.com/cve/CVE-2014-6287/) for this server

- Use `metasploit` to gain an initial shell

![Metasploit Attack](screenshots/2022-08-03-18-54-41.png)

- Gained initial shell and found user flag 

![Found User flag](screenshots/2022-08-03-18-56-56.png)

## Privilege Escalation

- Downloaded [PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1) and then uploaded it using metasploit `upload` comamnd

![PowerUp.ps1 Upload](screenshots/2022-08-03-19-10-49.png)

- Execute using Meterpreter, using **load powershell** and **powershell_shell**

![load powershell](screenshots/2022-08-03-19-19-33.png)

- Ran PowerUp.ps1

![Running PowerUp.ps1](screenshots/2022-08-03-20-05-15.png)

- Found a service with `CanRestart` option set to true. This means we can also write to the directory, replacing it with a malicious one, restart the service and run the infected program. 

![Vulnerable Service](screenshots/2022-08-03-20-16-16.png)

- Use `msfvenom` to create a reverse shell payload as a Windows executable 

![Msfvenom payload](screenshots/2022-08-03-20-27-15.png)

- Setup reverse shell handler

![Reverse shell setup](screenshots/2022-08-03-20-28-20.png)

- Stop legitimate service 

![Stopping legitimate service](screenshots/2022-08-03-20-32-10.png)

- Upload malicious payload 

![Uploading malicious payload](screenshots/2022-08-03-20-34-16.png)

- Start service again

![Start service again](screenshots/2022-08-03-20-35-53.png)

- Once the service is started this should spawn another meterpreter session with the listener we created 

![Reverse Shell Session](screenshots/2022-08-09-22-47-25.png)
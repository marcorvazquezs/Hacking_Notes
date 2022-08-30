# HackPark

## Initial Scan

- Ran nmap against the target machine

    ![Nmap Scan](screenshots/2022-08-24-16-52-09.png)

## Hydra Brute-force

- Use `Burp Suite` to identify the request type the website login forms uses

    ![Burp Suite](screenshots/2022-08-24-16-53-20.png)

- Now that we know it uses a `POST` request, we can use `HYDRA` to brute force using this command
  ![Hydra Brute Force](screenshots/2022-08-24-21-44-31.png)

  ![Logged into portal](screenshots/2022-08-29-17-10-25.png)

- Once logged in we can identify the version of BlogEngine (3.3.6.0)
- Use exploit-db.com to find vulnerabilities for this version of BlogEngine
  - Found [CVE](https://www.exploit-db.com/exploits/46353)

- Download the exploit and name the file `PostView.ascx` 
- Upload it to one of the posts. 

    ![Exploit Upload](screenshots/2022-08-29-17-24-37.png)

- Set up a listener using `nc -nlvp 4445`
- Navigated to the URL in the exploit and received a call back on the reverse shell

    ![Reverse Shell](screenshots/2022-08-29-17-36-09.png)

- Found who is the webserver running as using `whoami` command
- Setup meterpreter listener
    ![Meterpreter listener](screenshots/2022-08-29-17-54-20.png)
- Download payload
    ![Payload download](screenshots/2022-08-29-17-54-30.png)
- Gaining Meterpreter shell 
    ![Gained Meterpreter shell](screenshots/2022-08-29-17-55-40.png)
- Executing malicious payload
    ![Executing payload](screenshots/2022-08-29-17-55-53.png)
- Gaining additional system information
    ![Using sysinfo to gain information](screenshots/2022-08-29-17-56-29.png)
- Uploading winPEAS.exe
    ![Uploading winPEAS.exe](screenshots/2022-08-29-18-03-12.png)
- Investigating abnormal services
    ![Found SystemScheduler service](screenshots/2022-08-29-18-15-44.png)
- Generating malicious payload with `msfvenom`
    ![Msfvenom payload creation](screenshots/2022-08-29-18-18-39.png)
- Downloading `msfvenom` payload
    ![Payload download](screenshots/2022-08-29-18-22-58.png)
- Setup listener
    ![Listener setup](screenshots/2022-08-29-18-23-10.png)
- Finding user and root flags
    ![Finding flags](screenshots/2022-08-29-18-24-31.png)
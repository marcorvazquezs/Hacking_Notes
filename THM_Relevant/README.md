# Relevant

## Scope of Work

- The client has asked that you secure two flags (no location provided) as proof of exploitation:
  - User.txt
  - Root.txt

- Scope allowances:
  - All tools permitted but attempt manual exploitation first
  - Locate and note all vulnerabilities found
  - Submit the flags discovered to the dashboard
  - Only the IP address assigned to your machine is in scope
  - Find and report ALL vulnerabilities

## Reconaissance

- Nmap Scan Results
  ![Nmap Results 1](screenshots/2022-09-16-12-52-45.png)
  ![Nmap Results 2](screenshots/2022-09-16-12-53-16.png)

- Navigating to the IP address on a browser, found IIS is running

    ![IIS Splash Screen](screenshots/2022-09-16-12-36-01.png)

- SMBMAP results

    ![SMBMAP](screenshots/2022-09-16-13-00-49.png)

- SMB Vulnerability Scan resulted in finding target is vulnerable to `CVE-2017-0143`

  ![SMB Vuln Results](screenshots/2022-09-17-12-16-52.png)

## Initial Access

- Connected to the `nt4wrksv` share and found a `passwords.txt` file

    ![SMB share](screenshots/2022-09-16-14-01-59.png)

- Got a copy of this file and found encoded passwords

    ![Encoded Passwords](screenshots/2022-09-16-14-03-08.png)

- Decoded the passwords

    ![Decoded Passwords](screenshots/2022-09-17-11-42-41.png)

- Created a payload using msfvenom

    ![Msfvenom payload](screenshots/2022-09-17-13-21-53.png)

- Meterpreter listener

    ![Listener](screenshots/2022-09-17-13-22-27.png)

- Payload upload and call

    ![Payload Upload](screenshots/2022-09-17-13-23-10.png)

- Found the user flag

    ![User flag](screenshots/2022-09-17-13-25-36.png)

- Check privileges

    ![Privileges](screenshots/2022-09-17-13-27-45.png)

- Using the [PrintSpoofer](https://github.com/dievus/printspoofer) exploit to gain privilege escalation
- Uploaded the `PrintSpoofer.exe` and navigated to the share in `C:\inetpub\wwwroot\nt4wrksv` to run it using the `PrintSpoofer.exe -i -c cmd` command

    ![PrintSpoofer.exe](screenshots/2022-09-17-16-34-35.png)

    ![Root Shell](screenshots/2022-09-17-16-41-32.png)

- Found root flag

    ![Root Flag](screenshots/2022-09-17-16-43-51.png)

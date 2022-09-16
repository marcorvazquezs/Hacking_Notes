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

## Initial Access
- Connected to the `nt4wrksv` share and found a `passwords.txt` file

    ![SMB share](screenshots/2022-09-16-14-01-59.png)

- Got a copy of this file and found encoded passwords

    ![Encoded Passwords](screenshots/2022-09-16-14-03-08.png)

- Decoded the passwords
- 
# Analytics - HTB

***

## Scanning and Enumeration

- Running nmap scans

    ![Nmap Results](screenshots/2024-01-30-18-32-48.png)

- Running `ffuf`

- Found this login screen

    ![Metabase login screen](screenshots/2024-01-30-18-33-23.png)

***

## Research

- https://infosecwriteups.com/cve-2023-38646-metabase-pre-auth-rce-866220684396

***

## Initial Access

- Found the setup token using burp suite based on the previous article

    ![setup token](screenshots/2024-01-30-19-01-41.png)

- Found a metasploit exploit for this

    ![Metasploit exploit](screenshots/2024-01-30-20-57-23.png)

***

## Privilege Escalation

- Transferred linpeas.sh and found some interesting info in the environment section

    ![](screenshots/2024-01-30-21-39-31.png)

# Devvortex - HTB

***

## Scanning and Enumeration

- Running nmap scans

    ![Nmap Results](screenshots/2024-02-01-18-00-48.png)

- Found a subdomain

    ![subdomain results](screenshots/2024-02-01-20-57-48.png)

- Navigated to `dev.devvortex.htb` and tried `robots.txt`

    ![Robots.txt](screenshots/2024-02-01-20-58-43.png)

- Found a login screen at `/administrator`

    ![Joomla login](screenshots/2024-02-01-20-59-25.png)

- Ran `joomscan` against this to find more information

    ![joomscan results](screenshots/2024-02-01-21-00-08.png)

- From here found Joomla version `4.2.6` among other things

***

## Research

- https://github.com/Acceis/exploit-CVE-2023-23752
- https://github.com/diego-tella/CVE-2023-1326-PoC
- https://diegojoelcondoriquispe.medium.com/cve-2023-1326-poc-c8f2a59d0e00

***

## Initial Access

- Ran the exploit against `http://dev.devvortex.htb`

    ![Exploit](screenshots/2024-02-02-13-54-04.png)

- These credentials allowed me to login to the joomla admin page

    ![Joomla admin](screenshots/2024-02-02-14-04-38.png)

- Right off the bat we see this is running PHP so let's try to find where we can run some php code to get a shell

***

## Privilege Escalation

- Logged into the local joomla db using the credentials found for `lewis`

    ![Database search](screenshots/2024-02-02-14-33-58.png)

- Navigated to `joomla` database and found the table `sd4fg_users`. In there I found some hashes for `logan`

    ![Logan hashes](screenshots/2024-02-02-14-34-57.png)

- Ran this through john the ripper and got a password
- Using those credentials, I was able to login as `logan`
- Found the `user.txt`
- Ran `sudo -l` and found that `logan` can run `apport-cli`
- Did some investigation and found a PoC for this privilege escalation (linked in resources)

    ![apport-cli](screenshots/2024-02-02-14-50-40.png)

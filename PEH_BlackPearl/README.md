# Blackpearl - Provided by TCM 

- Box has ip address `10.0.2.14`

## Scanning and Enumeration

- Running `nmap` against target

    ![Nmap Results](screenshots/2022-03-09-20-27-16.png)

    - Interesting Findings
      - `22/tcp - open - ssh - OpenSSh 7.9p1 Debian (protocol 2.0)`
      - `53/tcp - open - domain ISC BIND 9.11.5-P4-5.1 (Debian Linux)`
      - `80/tcp - open - http nginx 1.14.2`

- Running `nikto` against the target

    ![nikto results](screenshots/2022-03-09-20-31-05.png)

- Running `dirbuster` against the target - no interesting results there 

- Navigated to `http://10.0.2.14` and found the nginx default page 

    ![Nginx landing page](screenshots/2022-03-09-20-32-05.png)

- Ran `gobuster` against the target and found: 

    ![gobuster results](screenshots/2022-03-09-20-45-37.png)

- Performing `dnsrecon` 

    ![dnsrecon results](screenshots/2022-03-10-16-34-31.png)

- Added `blackpearl.tcm` to `/etc/hosts/` 

    ![/etc/hosts](screenshots/2022-03-10-16-39-32.png)

- Navigated to `blackpearl.tcm`

    ![Results](screenshots/2022-03-10-16-40-10.png)

- Performed more directory brute forcing with `ffuf`

    ![ffuf results](screenshots/2022-03-10-16-45-00.png)

    ![navigate](screenshots/2022-03-10-16-46-15.png)

- Found `navigate` from `ffuf` so going to `blackpearl.tcm/navigate`

    ![Navigate CMS](screenshots/2022-03-10-16-53-53.png)

## Research

- Researched `Navigate CMS v2.8` exploits and found the following: 

    ![Navigate exploits](screenshots/2022-03-10-16-55-08.png)

## Exploitation

- Searching `metasploit` for the module 

    ![exploit module](screenshots/2022-03-10-16-56-39.png)

- Attempting to use module to attack the target

    ![Running exploit](screenshots/2022-03-10-16-59-59.png)

- Received a shell - moving on to privilege escalation 

    ![Shell](screenshots/2022-03-10-17-01-23.png)

## Privilege Escalation

- Spawn a TTY shell using `python -c 'import pty; pty.spawn("/bin/bash")'`

    ![TTY shell](screenshots/2022-03-10-17-07-09.png)

- Grabbed `linpeas.sh` from my attaching machine 

    ![Grabbing linpeas.sh](screenshots/2022-03-10-17-03-51.png)

- Found an `(Unknown SUID binary)`

    ![linpeas results](screenshots/2022-03-10-17-16-12.png)

- Searched `gtfobins` for using `php` with `SUID` and found the following 

    ![SUID results](screenshots/2022-03-10-18-00-46.png)

- Going to attempt this on the target 

    ![Shell](screenshots/2022-03-10-18-11-10.png)

- Got root shell


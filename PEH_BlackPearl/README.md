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

## Research 

## Exploitation 

## Privilege Escalation
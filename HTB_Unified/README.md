# TITLE HEADER

***

## Scanning and Enumeration

- Run nmap on the machine

***

## Research

- https://systemweakness.com/write-up-hack-the-box-starting-point-unified-tier-2-d4f3585320a1
- https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi

***

## Initial Access

- Get an interactive shell

    ![Interactive shell](screenshots/2024-01-18-17-44-50.png)

- Find mongo running and the default database unifi uses

    ![mongo db](screenshots/2024-01-18-17-58-00.png)

- Created my own SHA-512 hash and updated the database for the admin user
  
    ![Update admin password](screenshots/2024-01-18-18-23-30.png)

- Login as admin to the portal

***

## Privilege Escalation

- Went to Settings > Site and saw there is an SSH authentication for the user `root` and the password

    ![SSH Authentication](screenshots/2024-01-18-18-24-38.png)

- After this we can SSH to the machine as root

    ![SSH as root](screenshots/2024-01-18-18-25-12.png)

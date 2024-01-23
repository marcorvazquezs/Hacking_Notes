# Monitored - HTB

*** 

## Scanning and Enumeration

- Ran multiple nmap scans and got the following results

    ![Nmap Results 1](screenshots/2024-01-22-16-24-36.png)
    ![Nmap Results 2](screenshots/2024-01-22-16-24-59.png)

- Found the following login screen
  
    ![Nagios XI login screen](screenshots/2024-01-22-16-25-38.png)

- Found the following using ffuf

    ![ffuf results](screenshots/2024-01-22-18-06-31.png)

- Running ffuf again against `api`

    ![ffuf api results](screenshots/2024-01-22-18-28-37.png)

- Running ffuf again against `api/v1/`
  - This resulted in a bunch of results but all where giving a size of 32 so used `-fs 32` to filter those out

- Run `snmpwalk` and found the following

    ![snmpwalk results](screenshots/2024-01-22-18-28-02.png)

***

## Research

- [SNMP Brute Force](https://book.hacktricks.xyz/generic-methodologies-and-resources/brute-force#snmp)

    ![Nmap snmp brute force](screenshots/2024-01-22-17-11-41.png)

- [Nagios XI vulnerabilities](https://outpost24.com/blog/nagios-xi-vulnerabilities/)

*** 

## Initial Access

- Ran a post command on `/api/v1/authenticate` and the used the creds obtained from `snmpwalk`

    ![XPOST command](screenshots/2024-01-22-19-49-26.png)

- Obtained an auth token so went to login screen and passed on the token parameter, got logged in

    ![Nagios logged in](screenshots/2024-01-22-19-50-11.png)

- Found a sql injection

    ![SQLmap](screenshots/2024-01-22-20-13-25.png)

***

## Privilege Escalation

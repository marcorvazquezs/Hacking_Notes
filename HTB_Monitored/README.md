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

- Found an API key
  
    ![API Key](screenshots/2024-01-23-17-04-45.png)

***

## Privilege Escalation

- Let's use the API key found to create an account for us

    ![Creating an admin account](screenshots/2024-01-23-17-16-22.png)

- Logged in as the account we created

    ![MV account](screenshots/2024-01-23-17-17-58.png)

- Edited one of the commands to run a reverse shell connection

    ![Reverse Shell command](screenshots/2024-01-23-17-33-34.png)

- Navigated to Monitoring > Services and ran the `acaard` check command
- Got a shell connecting to the listener

    ![Shell](screenshots/2024-01-23-17-34-28.png)

- `user.txt` was waiting there!

    ![user flag](screenshots/2024-01-23-18-35-41.png)

- Got `linpeas.sh` onto the machine and ran it to find escalation paths

    ![linpeas.sh results](screenshots/2024-01-23-17-54-24.png)

- Removed the `npcd` service and created a new file that starts a shell

    ![npcd hijack](screenshots/2024-01-23-18-28-50.png)

- Started the listener and restarted the service using the `managed_services.sh`

    ![npcd restart](screenshots/2024-01-23-18-29-48.png)
    ![root shell](screenshots/2024-01-23-18-30-05.png)

- Found root flag

    ![Root flag](screenshots/2024-01-23-18-31-37.png)

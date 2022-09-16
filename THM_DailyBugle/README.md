# Daily Bugle

# Joomla Recon

- Found the Joomla version by going to this URL: http://10.10.185.111/administrator/manifests/files/joomla.xml

    ![Joomla Version](screenshots/2022-09-07-18-41-25.png)

- Found the following vulnerabilities
  
    ![Joomla Vulnerabilities](screenshots/2022-09-07-19-09-18.png)

- Downloaded a python script to exploit one of the vulnerabilities from here: https://github.com/XiphosResearch/exploits/tree/master/Joomblah

    ![Running Script](screenshots/2022-09-07-19-09-58.png)

- Found the following information (user, email and hash)

    ![User information](screenshots/2022-09-07-19-13-02.png)

- Time to crack the hash using john the ripper
- Once we have the password, use this to login to the joomla portal.

    ![Joomla portal](screenshots/2022-09-14-18-25-26.png)

- Time to get a reverse shell from [here](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
- Load the `reverse php` shell code onto the index.php file and then reload the website. 
- At the same time start a listener using `nc -nlvp 4445` to get a shell

    ![PHP Reverse Shell](screenshots/2022-09-14-18-35-07.png)

- The shell we got is for username `apache` and has no sudo privileges

    ![Apache Shell](screenshots/2022-09-14-18-39-32.png)

- Also was not able to switch user `jjameson` using the password we cracked

- Found nothing on crontab useful to escalate
- Next step, let's look at web files in `/var/www/html/`

    ![HTML contents](screenshots/2022-09-14-18-41-36.png)

- Found a `web.config.txt` and `configuration.php` files that looks interesting
- `configuration.php` contains some passwords!

    ![Configuration.php](screenshots/2022-09-14-18-44-22.png)

- Tried this to switch to root but it failed. 
- Tried it for user `jjameson` and that works!
- Figured out the yum escalation from [GTFOBins](https://gtfobins.github.io/gtfobins/yum/)

    ![Yum Escalation](screenshots/2022-09-14-20-26-35.png)
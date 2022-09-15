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
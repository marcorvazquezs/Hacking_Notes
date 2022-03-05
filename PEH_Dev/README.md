# Dev Box - Provided by TCM Security

- Box is using 10.0.2.11

## Scanning and Enumeration

- Running nmap against target

  ![Nmap Results](screenshots/2022-03-05-08-33-18.png)    

- Interesting Findings:
  - Port `22/tcp` open - SSH - OpenSSH 7.9p1 Debian (protocol 2.0)
  - Port `80/tcp` open - http - Apache httpd 2.4.38 ((Debian))
  - Port `111/tcp` open - rpcbind 2-4
  - Port `2049/tcp` open - nfs_acl 3 
  - Port `8080/tcp` open - http - Apache httpd 2.4.38 ((Debian))
    - `http-title`: PHP 7.3.27-1

- Running `nikto` since found Apache is running on the target

    ![Nikto Results](screenshots/2022-03-05-08-37-24.png)


- Navigated to the site `http://10.0.2.11` and found this page

    ![Bolt CMS](./screenshots/2022-03-05-08-04-08.png)

- Navigated to `http://10.0.2.11:8080` and found this page

    ![PHP information](screenshots/2022-03-05-08-38-36.png)

    ![Apache Information](screenshots/2022-03-05-08-46-27.png)

- Ran `dirbuster` against site

  ![Dirbuster](screenshots/2022-03-05-08-47-32.png)

- Interesting findings 
  - `/index.php`
  - `/public/index.php`

- Navigated to `/public/index.php` page which was found using `dirbuster` and got this page 

  ![Bolt User page](screenshots/2022-03-05-08-56-14.png)

  - Interesting Findings 
    - `Bolt 3.7.2`
    - Seems like we can create a user and get root

- Listed `nfs` file share 

  ![File Share](screenshots/2022-03-05-09-16-55.png)

- Mounted file share found on `/srv/nfs` and found a `save.zip`

  ![Mounted file share](screenshots/2022-03-05-09-18-34.png)

- Navigated to `/app/config` and found a `config.yml` file 

  ![Config.yml](screenshots/2022-03-05-09-28-15.png)

- Navigated to `http://10.0.2.11:8080/dev/` and found this screen

  ![BoltWire](screenshots/2022-03-05-09-35-51.png)

## Research

- What is Bolt? 
  - Looks like Bolt is CMS software - [Bolt CMS](https://boltcms.io/)
  
- Researching vulnerabilities for `PHP Version 7.3.27-1` 
  - [PHP Vulnerability: CVE-2021-21702](https://www.rapid7.com/db/vulnerabilities/php-cve-2021-21702/)

- Researching vulnerabilities for `Bolt CMS` using `searchsploit` and `Metasploit`

    ![Bolt Vulnerabilities](screenshots/2022-03-05-08-48-25.png)

    ![Metasploit Results](screenshots/2022-03-05-08-48-50.png)

- Researching vulnerabilities for `Apache/2.4.38 (Debian)`
  - [Vulmon](https://vulmon.com/searchpage?q=apache+http+server+2.4.38)

  ![Searchsploit Result](screenshots/2022-03-05-08-51-04.png)

- Researching vulnerabilities for `BoltWire` 
  - [BoltWire Local File Inclusion](https://www.exploit-db.com/exploits/48411)

## Exploitation

### Bolt
- Trying to create a user using the page found at `/public/index.php`
- This was successful but not sure what I can do from here. 

  ![Admin Page](screenshots/2022-03-05-09-07-58.png)

- Tried using the `unix/webapp/bolt_authenticated_rce` module but this failed 

  ![Failed module attempt](screenshots/2022-03-05-09-37-27.png)

### File Share
- Attempt to crack file share using `fcrackzip`

  ![fcrackzip](screenshots/2022-03-05-09-21-23.png)

- Found the password so using that to unzip the file
- Zip file contained: 

  ![Zip file contents](screenshots/2022-03-05-09-24-42.png)

### BoltWire 
- Attempting local file inclusion on the page found
- First created a user so that we are authenticated
- Pasted `index.php?p=action.search&action=../../../../../../../etc/passwd` at the end of the URL and it worked. We got the contents of `/etc/passwd` 

  ![Local File inclusion](screenshots/2022-03-05-09-40-07.png)

  - Found a user `jeanpaul` which we found before as `jp`


### SSH 
- Putting together the information so far we can try to SSH to the machine 
  - User: `jeanpaul` 
  - Found a line in the `todo.txt` that says `jeanpaul` loves `java` and a password in the `config.yml` file of `I_love_java` so I'm going to give that a shot.
  - Going to use the `id_rsa` file we found

  ![JeanPaul shell ](screenshots/2022-03-05-09-44-45.png)

  - This worked so now we have a shell on the target

# Privilege Escalation

- Do a `sudo` check to see what we can run without password

  ![Sudo Check ](screenshots/2022-03-05-09-48-15.png)

- Found we can run `sudo /usr/bin/zip` with `NOPASSWD`
- Went to [GTFOBins](https://gtfobins.github.io/) to figure out how to leverage this

  ![GTFOBins Result](screenshots/2022-03-05-09-51-07.png)

- Attempting this on the target

  ![Sudo Zip](screenshots/2022-03-05-09-53-59.png)

- Box was rooted! 




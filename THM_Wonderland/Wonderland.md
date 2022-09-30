# Wonderland Room

## Recon

- Nmap Scan

    ![Nmap Scan Results](screenshots/2022-09-28-19-49-03.png)

- Dirsearch results
    - `/r`
    - `/img`


- Download the `white_rabbit_1.jpg` file to see if there is anything hidden

    ![Steg image](screenshots/2022-09-28-19-52-42.png)

- Navigate to that URL

    ![URL](screenshots/2022-09-28-19-53-33.png)

- Viewed page source and found some credentials
- Use these to ssh to the box
- We are in! 

- Found the `user.txt` flag in `/root/` directory
- Used `sudo -l` and found we can run a script called `walrus_and_the_carpenter.py` as user `rabbit`
- Found the script in question calls the `random` library but has no absolute path. 
- We create a file called `random.py` that contains the following code
  ```python
  import os
  os.system("/bin/bash")
  ```
- Now we are user `rabbit`

- We find the following in the `/home/rabbit/` directory

    ![Rabbit directory contents](screenshots/2022-09-29-16-55-36.png)

- We get the `teaParty` file back to Kali and do some inspection and found the following

    ![teaParty file](screenshots/2022-09-29-17-01-12.png)

- This calls the `date` command without an absolute path, so we can create a new `date` file to take advantage of this
- Created a fake `date` file that contains the following and made it executable
  ```
  #!/bin/bash
  /bin/bash
  ```

- Ran `./teaParty` again and now we are `hatter`
- There we also find a `password.txt` file

    ![Hatter directory](screenshots/2022-09-29-17-09-34.png)

- SSH to the machine using username `hatter` and the password found
- Uploaded `linpeas.sh` from my kali machine using `python3 -m http.server`
- Found that perl has `cap_setuid+ep` capability set

    ![capablities](screenshots/2022-09-29-17-39-29.png)

- Exploit this to gain root using
  `./perl -e 'use POSIX (setuid); POSIX::setuid(0); exec "/bin/bash";'`

- We get `root` and can now get the `root.txt` flag in `/home/alice`

    ![Root shell](screenshots/2022-09-29-17-40-56.png)

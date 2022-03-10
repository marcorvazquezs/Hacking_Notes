# Butler - Provided by TCM

- Target has IP address of `10.0.2.80`

## Scanning and Enumeration

- Running nmap against target

    ![Nmap Results](screenshots/2022-03-07-19-43-41.png)

    - Interesting findings 
      - `135/tcp - open - msrpc`
      - `139/tcp - netbios-ssn`
      - `445/tcp - microsoft-ds`
      - `5040/tcp - unknown`
      - `7680/tcp - pando-pub`
      - `8080/tcp - http - Jetty 9.4.41.v20210516`
      - `49664-49668/tcp - msrpc`
      - `49670/tcp - msrpc`

- Running `nikto` against the target on port `8080`

    ![Nikto Results](screenshots/2022-03-07-20-14-21.png)

- Navigated to `http://10.0.2.80:8080` and got this page. 

    ![Jenkins Login](screenshots/2022-03-07-20-15-18.png)

## Research

- Researching Jenkins exploits 
  - [pwn_jenkins](https://github.com/gquere/pwn_jenkins)

- Didn't find much else


## Exploitation

- Going to attempt to brute force the Jenkins login page using Burp Suite 

    ![Burp Suite](screenshots/2022-03-07-20-28-48.png)

    ![Payload](screenshots/2022-03-07-20-29-15.png)

    ![Cluster Bomb attack](screenshots/2022-03-09-18-04-47.png)
    - Seems like username: `jenkins` and password: `jenkins` works

    - Using [Groovy Reverse Shell](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76) on the Script Console
  
    - Set `netcat` to listen on port `8044` and got a shell 

    ![Reverse Shell](screenshots/2022-03-09-18-15-34.png)

## Privilege Escalation

- Downloaded `winpeas.exe` and host a transfer file on my attack box 

    ![Winpeas](screenshots/2022-03-09-18-28-46.png)

- Navigated to the `butler` user directory

    ![Moved to butler user directory](screenshots/2022-03-09-18-27-41.png)

- Grabbed the `winpeas.exe` from the attacking machine using `certutil.exe`

    ![Grabbed winpeas.exe](screenshots/2022-03-09-18-35-42.png)

- Found an unquoted and unspaced service 

    ![Services](screenshots/2022-03-09-18-55-52.png)

- Generated a malicious reverse shell file and named it `Wise.exe` using `msfvenom`

    ![msfvenom](screenshots/2022-03-09-18-58-30.png)

- Set `netcat` to listen on port `7777`

- Get the `Wise.exe` we created 

    ![Wise.exe](screenshots/2022-03-09-19-10-33.png)

- Stop the running program `WiseBootAssistant` and confirm it is stopped 

    ![Stopped WiseBootAssistant](screenshots/2022-03-09-19-11-44.png)

- Run our `Wise.exe` and it will execute as system 

    ![start wisebootassistant](screenshots/2022-03-09-19-13-10.png)

- We got a shell on port `7777` and see it is an elevated shell 

    ![Root shell](screenshots/2022-03-09-19-13-23.png)
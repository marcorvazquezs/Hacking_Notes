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

## Privilege Escalation
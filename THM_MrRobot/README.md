# Mr. Robot - Provided by TryHackMe

---

- Box is using 10.10.67.131

## Scanning

- Using nmap to scan for running services and ports

    ![Nmap Results](screenshots/2022-04-08-16-57-51.png)

## Investigation

- Navigated to the `/robots.txt` file and found the following which gave me the first key

![robots.txt](screenshots/2022-04-08-17-18-00.png)

- Went ahead and downloaded the `fsocity.dic` file
- The file seems to be a username list

- Using burp suite, figured out the username and password are passed in the POST request

    ![Burp Suite](screenshots/2022-04-08-17-28-26.png)

- Then using hydra and the `fsocity.dic` file, conduct a brute force attack

    ![Hydra attack](screenshots/2022-04-08-17-29-46.png)

- This returned with username `Elliot`
    ![Username Discovered](screenshots/2022-04-08-17-30-30.png)

- Running the same attack with the known username `Elliot`

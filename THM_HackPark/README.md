# HackPark

## Initial Scan 
- Ran nmap against the target machine 

    ![Nmap Scan](screenshots/2022-08-24-16-52-09.png)

## Hydra Brute-force 

- Use `Burp Suite` to identify the request type the website login forms uses 

    ![Burp Suite](screenshots/2022-08-24-16-53-20.png)

- Now that we know it uses a `POST` request, we can use `HYDRA` to brute force 

`
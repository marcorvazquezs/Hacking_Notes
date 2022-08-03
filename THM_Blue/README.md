# Blue

## Recon

- Ran nmap against the box

![Nmap Results](screenshots/2022-08-01-18-32-29.png)

## Gain Access 

- Start metasploit
- Search for `ms17_010`

![Exploit Search](screenshots/2022-08-01-18-36-56.png)

![Show and set options](screenshots/2022-08-01-18-38-20.png)

![Run exploit](screenshots/2022-08-01-18-42-38.png)

## Escalate

- Search to `shell_to_meterpreter` to turn shell to a meterpreter shell 

![Set Options and run](screenshots/2022-08-01-18-46-56.png)

- Once done switch to the session

![Elevated Shell](screenshots/2022-08-01-18-52-52.png)

- Use `ps` to list the processes

![PS command](screenshots/2022-08-01-18-59-12.png)

- Use `migrate PID` to migrate to a process running under `NT AUTHORITY\SYSTEM`

## Cracking
- Use `hashdump` to dump all of the passwords on the machine

![Hashdump](screenshots/2022-08-01-19-06-02.png)

- Store passwords in the database

![Storing hashes](screenshots/2022-08-01-19-22-03.png)

- Use `auxiliary/analyze/crack_windows` to crack hashes

![Crack Hashes](screenshots/2022-08-01-19-23-49.png)
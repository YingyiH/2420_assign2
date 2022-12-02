# 2420_assign2
2420_assignment2

## Step 1: Set up 
- Create a VPC:
  - Get into “Network” → Choose “VPC”
  - Choose “San Francisco SFO3” → Choose “Generate an IP range for me”
  - Set a name of VPC to be “vpc 2420”
    ![image](https://user-images.githubusercontent.com/100324443/205243485-e4aa0f7d-89e7-4040-ad3b-62b11583ab86.png)
- Create droplets `server-01` and `server-02`:
  - Choose “San Francisco” → Choose VPC to be “vpc-2420”
  - Choose everything else as same as creating droplets as before, except for setting two droplets and name them at the end. Also, give this droplet a tag
  DONE:
      ![image](https://user-images.githubusercontent.com/100324443/205243758-5a15a543-3b89-4ec1-b84b-ae8644bfcdcd.png)
- Create a load-balancer:
  - Choose “San Francisco” → Choose VPC to be “vpc-2420”
  - Select the tag “Web”
  DONE:
      ![image](https://user-images.githubusercontent.com/100324443/205243967-a329c4b7-cb06-40d9-b6c0-1b792c02f79c.png)
- Create a Firewall in Digital Ocean:
  - Not change anything of the first rule in the Inbound Rules (set it as default)
  - Create a new HTTP rule and set the source to be the load-balancer that we just created
  - Not change anything of the Outbound Rules
  - Select the tag to be “Web”
  DONE: 
      ![image](https://user-images.githubusercontent.com/100324443/205244165-2eafc52f-abe4-4c31-a76d-af6c61dd2533.png)

## Step 2: Set up for `server-01` and `server-02`
- Log in:
  - Log in server-01
  ```
  ssh -i /Users/doridori/.ssh/<private key name> root@<server-01 IP>
  ```
    ![image](https://user-images.githubusercontent.com/100324443/205245334-b834b4ca-b868-446e-9204-5a4fc45143fd.png)
    ![image](https://user-images.githubusercontent.com/100324443/205245361-b5fa86d2-8722-4b5c-93a7-51b59e118f1a.png)

  - Log in server-02
  ```
  ssh -i /Users/doridori/.ssh/<private key name> root@<server-02 IP>
  ```
    ![image](https://user-images.githubusercontent.com/100324443/205245504-dd6815b6-f6b2-41c0-a3e3-f33642e5c0ff.png)
    ![image](https://user-images.githubusercontent.com/100324443/205245523-b50eeda0-43b0-48cf-a5a0-abe05f93b850.png)

- Create new server in both `server-01` and `server-02`:
  - Add new user: `useradd -ms /bin/bash <username>`
  - Add user to sudo: `usermod -aG sudo <username>`
  - Change new user password: `passwd <username>`
  - Log in as new user: `su <username>`
- Copy ssh public key in `server-01` and `server-02` to connect Mac and servers:
  - Use `cd` to open directory `.ssh` in Mac
  - Use `cat` to show `<key name>.pub`
  - Copy public key
  - Create `.ssh` directory in `/home/<username>` directory in `server-01` and `server-02`
  - Use `vim /home/<username>/.ssh/authorized_keys` to create an authorized_keys file
  - Paste the public key in `authorized_keys` file

## Step 3: Install `Caddy` web server and create `.volta` directory
- Update in both server-01 and server-02:
  - Use `sudo apt update` to update the package cache
  - Use `sudo apt upgrade` to update the package to new version
- Install web server in server-01 and server-02 (Install Caddy):
  - Use `wget https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz tar xvf https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz` to download the caddy file in both `server-01` and `server-02`
    ![image](https://user-images.githubusercontent.com/100324443/205251317-d57c9b8b-d782-4a58-abdf-e0c6bdb0915c.png)

  - Check if caddy files downloaded
    ![image](https://user-images.githubusercontent.com/100324443/205251467-c12d5b62-eaa7-4e1c-9cd9-a789f8dfa7ea.png)

  - Copy caddy file to bin directory
    - Use `sudo chown root: caddy` to change the caddy file’s owner and group to root
    - Use cp to copy caddy to /usr/bin.
       ![image](https://user-images.githubusercontent.com/100324443/205252553-06f460fa-6883-4b8a-9d55-5fb460d8590c.png)

  - Use mkdir to create a .volta directory in `server-01` and `server-02`
  ![image](https://user-images.githubusercontent.com/100324443/205252690-2e1035bc-2c20-43cc-b939-cbaff7e6def6.png)
  
## Step 4: Write `index.html` and `index.js`
- Create relative directories in Mac:
  - Use `mkdir` to create new directory named "2420-assign-two"
  - Use `mkdir` to create two directories `html` and `src` in the directory `2420-assign-two`
      ![image](https://user-images.githubusercontent.com/100324443/205254032-de46da66-a60e-4c5e-80f9-3f4c51dfbaf7.png)
- Write files:
  - Write `index.html` in `html` directory
    - Use `vim /html/index.html` to write the html file
    
        <img width="609" alt="Screenshot 2022-12-02 at 12 55 41 AM" src="https://user-images.githubusercontent.com/100324443/205254478-2399c441-b24d-45ac-860d-7f15279a3dee.png">
  - Write `index.js` in `src` directory
    - Use `vim /src/index.js` to write the JavaScript file
    
        <img width="437" alt="Screenshot 2022-12-02 at 12 58 20 AM" src="https://user-images.githubusercontent.com/100324443/205255020-5a262183-b03e-4971-99de-7a1594e669e3.png">







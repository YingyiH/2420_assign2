# 2420_assign2
2420_assignment2

Student: Yingyi He
Student ID: A01230375

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
- Copy the files and directories to `server-01` and `server-02`:
  - Use `sftp` to connect the `local host` to `server-01` and `server-02`
  - Use `put -r` to transfer the directory `html` and `src`
      ![image](https://user-images.githubusercontent.com/100324443/205255883-295e1729-5ad7-411e-8574-206f44210b8d.png)
      ![image](https://user-images.githubusercontent.com/100324443/205255939-de2310c7-ace7-48bb-91ab-36f084bc75ae.png)
- Check if files and directories in `server-01` and `server-02`:
  - Log in `server-01` and `server-02`
  - Use `ls` to check if the files and directories already there
    ![image](https://user-images.githubusercontent.com/100324443/205256370-b83c30a3-c77f-41d0-90eb-bdfb1eb44087.png)
- Move the files and directories to correct location:
  - Create `www` directory in `/var`
  - Use `mv` to move the directories to `/var/www`
      ![image](https://user-images.githubusercontent.com/100324443/205256772-75ad1c8b-2676-476e-a58e-4749fca35012.png)

## Step 5: Write caddy file
- Get into `/etc` directory
- Use `mkdir` to create `caddy` directory in `server-01` and `server-02`
- Use `sudo vim /etc/caddy/Caddyfile` to write caddy file
    ![image](https://user-images.githubusercontent.com/100324443/205257422-d62e03a5-faa9-4de3-9e31-31aa2fd05c24.png)

## Step 6: Install `volta`, `node` and `npm`
- Install `volta`:
  - Use `curl https://get.volta.sh | bash` to install volta
      ![image](https://user-images.githubusercontent.com/100324443/205257824-82647199-b722-4931-b69a-46c21c781677.png)
  - Change `VOLTA_HOME` variable value:
    - Use `vim ~/.bashrc` to open .bashrc file to add variables in both `server-01` and `server-02`
         ![image](https://user-images.githubusercontent.com/100324443/205257971-9ec32c49-5300-49c9-84a2-89cd19afdde2.png)
    - Use `source ~/.bashrc` to apply for the new changes
        ![image](https://user-images.githubusercontent.com/100324443/205258036-08b9eaf5-6bf6-4ae3-9d5d-66f4a8fd1e07.png)
- Install `node`:
  - Use `volta install node` to install node
    ![image](https://user-images.githubusercontent.com/100324443/205258419-8ae7c19d-ea71-48f8-b081-ef0dc5a1ddc0.png)
- Install `npm`:
  - Use `npm init` to add `package.json` in `src` directory
    ![image](https://user-images.githubusercontent.com/100324443/205258794-cda8c54c-8a4c-4375-bb30-12cf98d581de.png)
  - Use npm install fastify to install fastify module
    ![image](https://user-images.githubusercontent.com/100324443/205258856-0e0a8bf1-16ff-44e7-9ae4-ee5eadc8ae02.png)

## Step 7: Write `caddy.service` and `hello_web.service`, and enable them
- Write service files:
  - Use `sudo vim /etc/systemd/system/caddy.service` to write `caddy.serivce`
    <img width="517" alt="Screenshot 2022-11-30 at 8 54 25 PM" src="https://user-images.githubusercontent.com/100324443/205259666-5ddce813-8d76-49ea-9ee8-19804dfe43e5.png">
  - Use `sudo vim /etc/systemd/system/hello_web.service` to write `hello_web.service`
    ![image](https://user-images.githubusercontent.com/100324443/205260088-16136b73-a3ba-4f9b-9edb-619a3e218d27.png)
- Enable and restart services:
  - Use `sudo systemctl daemon-reload` to reload
  - Use `sudo systemctl disable <server-name>` to disable services at first
  - Use `sudo systemctl enable <server name>` to enable services
  - Use `sudo systemctl restart <server name>` to restart services
  - Use `sudo systemctl status <server name>` to check if services are activated
    ![image](https://user-images.githubusercontent.com/100324443/205260787-9dadd185-9be6-4082-8a7a-7b942543627b.png)
    <img width="1470" alt="Screenshot 2022-12-02 at 1 28 37 AM" src="https://user-images.githubusercontent.com/100324443/205260920-74425a79-ef06-4636-a9c4-43b861e8b122.png">

## Step 8: Change file contents in `server-02` and test files
- Change owner and group of `index.html` in both `server-01` and `server-02`:
  
- Change file contents:
  - Change `index.html` content in `server-02`
      ![image](https://user-images.githubusercontent.com/100324443/205261889-239e875d-9dfb-425d-89fe-2fd9179fe9b9.png)
  - Change `index.js` content in `server-01` and `server-02`
      ![image](https://user-images.githubusercontent.com/100324443/205262099-286270c3-5d01-4c77-b1de-7a1286921d18.png)
      ![image](https://user-images.githubusercontent.com/100324443/205262141-a56aa2c1-2682-4c0b-a5a0-f0e1a3b4c961.png)
- Test web app and server blocks:
  - Run load-balancer IP address in browser
  - Refresh the browser to check if the page gets random switch
      ![image](https://user-images.githubusercontent.com/100324443/205262697-678bd921-dac0-42b8-a3ca-2abee3462f7b.png)
      ![image](https://user-images.githubusercontent.com/100324443/205262728-5a324a2d-fffe-473a-b3ab-4f0bc3479387.png)

## Step 9: Test load-balancer
![image](https://user-images.githubusercontent.com/100324443/205262697-678bd921-dac0-42b8-a3ca-2abee3462f7b.png)
![image](https://user-images.githubusercontent.com/100324443/205262728-5a324a2d-fffe-473a-b3ab-4f0bc3479387.png)
<img width="335" alt="Screenshot 2022-12-02 at 1 40 43 AM" src="https://user-images.githubusercontent.com/100324443/205263255-472e412e-bf2f-4cc2-a828-07ed6aa69078.png">
<img width="372" alt="Screenshot 2022-12-02 at 1 41 12 AM" src="https://user-images.githubusercontent.com/100324443/205263356-90488730-4550-4122-bc80-639a3014c032.png">




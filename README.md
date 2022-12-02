# 2420_assign2
2420_assignment2

## Author
- Student: Yingyi He
- Student: A01230375
- Set: Set B 

## Description
This repository contains files and directories in terms of building a virtual private cloud(VPC) to group cloud resources together. 
It contains a load balancer and firewall  redirect incoming traffic to two servers.
This Readme file mostly for MacOS user.

Check this IP address and keep refreshing to switch two website: http://24.199.71.1

## Table Content ##
- [2420_assign2](#2420-assign2)
  * [Author](#author)
  * [Description](#description)
  * [Table Content](#table-content)
  * [Step 1 - Set up](#step-1---set-up)
  * [Step 2 - Set up for server-01 and server-02](#step-2---set-up-for-server-01-and-server-02)
  * [Step 3 - Install Caddy web server and create .volta directory](#step-3---install-caddy-web-server-and-create-volta-directory)
  * [Step 4 - Write index.html and index.js](#step-4---write-indexhtml-and-indexjs)
  * [Step 5 - Write caddy file](#step-5---write-caddy-file)
  * [Step 6 - Install Volta Node and npm](#step-6---install-volta-node-and-npm)
  * [Step 7 - Write and enable caddy service and web app service](#step-7---write-and-enable-caddy-service-and-web-app-service)
  * [Step 8 - Change file contents in server-02 and test files](#step-8---change-file-contents-in-server-02-and-test-files)
  * [Step 9 - Test load-balancer](#step-9---test-load-balancer)

## Step 1 - Set up 
- Create a VPC:
  - Get into “Network” → Choose “VPC”
  - Choose “San Francisco SFO3” → Choose “Generate an IP range for me”
  - Set a name of VPC to be “vpc 2420”
  
     <img width="450" alt="create_VPC" src="https://user-images.githubusercontent.com/100324443/205277301-c357c8ab-7b57-443d-a0d3-066c7b1e8a47.png">

- Create droplets `server-01` and `server-02`:
  - Choose “San Francisco” → Choose VPC to be “vpc-2420”
  - Choose everything else as same as creating droplets as before, except for setting two droplets and name them at the end. Also, give this droplet a tag
  
     <img width="941" alt="create_two_servers" src="https://user-images.githubusercontent.com/100324443/205277256-f801cf53-3a9a-4f47-bbfa-bd79345c7c77.png">

- Create a load-balancer:
  - Choose “San Francisco” → Choose VPC to be “vpc-2420”
  - Select the tag “Web”
  
     <img width="600" alt="create_load_balancer" src="https://user-images.githubusercontent.com/100324443/205277328-521cb494-34b0-48c9-a5d9-95c40e2b2e56.png">

- Create a Firewall in Digital Ocean:
  - Not change anything of the first rule in the Inbound Rules (set it as default)
  - Create a new HTTP rule and set the source to be the load-balancer that we just created
  - Not change anything of the Outbound Rules
  - Select the tag to be “Web”
  
     <img width="942" alt="create_firewall" src="https://user-images.githubusercontent.com/100324443/205277753-89ff7efa-bdc7-4194-8db9-db3b1e9f484f.png">

## Step 2 - Set up for server-01 and server-02
- Log in:
  - Log in server-01
  ```
  ssh -i /Users/doridori/.ssh/<private key name> root@<server-01 IP>
  ```
  
     <img width="600" alt="server-01_IP" src="https://user-images.githubusercontent.com/100324443/205277846-533489db-1f20-4a95-ba18-b602440dcaf3.png">
     <img width="500" alt="login_server-01" src="https://user-images.githubusercontent.com/100324443/205277898-d4e3558e-3ae5-4cb9-a1a3-45693e760f96.png">

  - Log in server-02
  ```
  ssh -i /Users/doridori/.ssh/<private key name> root@<server-02 IP>
  ```
  
     <img width="600" alt="server-02_IP" src="https://user-images.githubusercontent.com/100324443/205278155-33ffb7cf-7f51-428a-b5cf-eeb52f14a023.png">
     <img width="500" alt="login_server-02" src="https://user-images.githubusercontent.com/100324443/205278178-6e28ca95-7c03-4a95-b572-2ca8aeab2b4a.png">

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

## Step 3 - Install Caddy web server and create .volta directory
- Update in both server-01 and server-02:
  - Use `sudo apt update` to update the package cache
  - Use `sudo apt upgrade` to update the package to new version
- Install web server in server-01 and server-02 (Install Caddy):
  - Use `wget https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz tar xvf https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz` to download the caddy file in both `server-01` and `server-02`
  
    <img width="970" alt="download_caddy" src="https://user-images.githubusercontent.com/100324443/205278259-2a97046f-9902-441d-b2d4-ef902afb3e1a.png">


  - Check if caddy files downloaded
  
    <img width="400" alt="check_caddy" src="https://user-images.githubusercontent.com/100324443/205278432-f33b6d26-597d-49e4-b13c-56df0e3dbd3b.png">

  - Copy caddy file to bin directory
    - Use `sudo chown root: caddy` to change the caddy file’s owner and group to root
    - Use cp to copy caddy to /usr/bin.
    
       <img width="500" alt="copy_caddy_to_usr_bin" src="https://user-images.githubusercontent.com/100324443/205278509-1964f5b8-97fe-40e2-be94-0ce9407fbe0c.png">


  - Use mkdir to create a .volta directory in `server-01` and `server-02`
       <img width="970" alt="create_volta_directory" src="https://user-images.githubusercontent.com/100324443/205278558-c015bb19-38f8-47ee-801f-305f518898a6.png">

  
## Step 4 - Write index.html and index.js
- Create relative directories in Mac:
  - Use `mkdir` to create new directory named "2420-assign-two"
  - Use `mkdir` to create two directories `html` and `src` in the directory `2420-assign-two`
  
      <img width="937" alt="create_html_src_directory" src="https://user-images.githubusercontent.com/100324443/205278759-2c2fcbfa-3ae4-4882-8c0f-9ebc5f16ef23.png">

- Write files:
  - Write `index.html` in `html` directory
    - Use `vim /html/index.html` to write the html file
    - 
      <img width="606" alt="server-01_index_html" src="https://user-images.githubusercontent.com/100324443/205278920-8c605d9d-4fd1-49c4-b5dc-f19e14b6a4cc.png">

  - Write `index.js` in `src` directory
    - Use `vim /src/index.js` to write the JavaScript file
    
      <img width="431" alt="index_js" src="https://user-images.githubusercontent.com/100324443/205278976-20c82d9e-2061-4d57-a125-99a707749cd6.png">

- Copy the files and directories to `server-01` and `server-02`:
  - Use `sftp` to connect the `local host` to `server-01` and `server-02`
  
    <img width="937" alt="sftp_connect" src="https://user-images.githubusercontent.com/100324443/205279075-6a8cf2db-51d7-4cf9-b871-25ceecbebed4.png">

  - Use `put -r` to transfer the directory `html` and `src`
  
    <img width="937" alt="sftp_put" src="https://user-images.githubusercontent.com/100324443/205279108-0d6ef28b-be89-4625-b410-97b0847985b0.png">

- Check if files and directories in `server-01` and `server-02`:
  - Log in `server-01` and `server-02`
  - Use `ls` to check if the files and directories already there
  
      <img width="935" alt="check_sftp_copy" src="https://user-images.githubusercontent.com/100324443/205279321-65f45d9a-f749-4a9d-a31f-82008d01c3f8.png">

- Move the files and directories to correct location:
  - Create `www` directory in `/var`
  - Use `mv` to move the directories to `/var/www`
  
    <img width="909" alt="move_copied_files" src="https://user-images.githubusercontent.com/100324443/205279362-5c09ad15-cc2b-4bd7-b76d-91f2218372ca.png">

## Step 5 - Write caddy file
- Get into `/etc` directory
- Use `mkdir` to create `caddy` directory in `server-01` and `server-02`
- Use `sudo vim /etc/caddy/Caddyfile` to write caddy file

  <img width="947" alt="Caddyfile" src="https://user-images.githubusercontent.com/100324443/205279429-c39c403a-7d70-4ba1-8c2c-375ac8a998f5.png">

## Step 6 - Install Volta Node and npm
- Install `volta`:
  - Use `curl https://get.volta.sh | bash` to install volta
  
     <img width="940" alt="install_volta" src="https://user-images.githubusercontent.com/100324443/205279504-d349c9eb-bf00-4173-a37f-ede762cf506d.png">

  - Change `VOLTA_HOME` variable value:
    - Use `vim ~/.bashrc` to open .bashrc file to add variables in both `server-01` and `server-02`
    
        <img width="899" alt="change_volta_variables" src="https://user-images.githubusercontent.com/100324443/205279544-36b81549-db03-484f-948b-2e16888de945.png">

    - Use `source ~/.bashrc` to apply for the new changes
    
        <img width="894" alt="source_variables" src="https://user-images.githubusercontent.com/100324443/205279582-6c2c7ce6-8550-40d2-a560-f3985eacdb3b.png">

- Install `node`:
  - Use `volta install node` to install node
  
     <img width="936" alt="install_node" src="https://user-images.githubusercontent.com/100324443/205279656-4f850b9f-70cf-4c80-9e66-47a0b2de3a01.png">

- Install `npm`:
  - Use `npm init` to add `package.json` in `src` directory
  
    <img width="937" alt="package_json" src="https://user-images.githubusercontent.com/100324443/205279710-3fde1a17-edfa-4a5f-8afa-5d4a78eb8f78.png">

  - Use npm install fastify to install fastify module
  
     <img width="926" alt="install_fastify" src="https://user-images.githubusercontent.com/100324443/205279736-4b163e38-4e1d-4634-b5a6-c31824119099.png">

## Step 7 - Write and enable caddy service and web app service
- Write service files:
  - Use `sudo vim /etc/systemd/system/caddy.service` to write `caddy.serivce`
  
     <img width="517" alt="caddy_service" src="https://user-images.githubusercontent.com/100324443/205280467-7e4340ee-4ada-4b0a-a15a-f0e607be16eb.png">


  - Use `sudo vim /etc/systemd/system/hello_web.service` to write `hello_web.service`
  
     <img width="935" alt="hello_web_service" src="https://user-images.githubusercontent.com/100324443/205279816-8af55e6e-2780-4b31-b8ec-22a01761ff49.png">

- Enable and restart services:
  - Use `sudo systemctl daemon-reload` to reload
  - Use `sudo systemctl disable <server-name>` to disable services at first
  - Use `sudo systemctl enable <server name>` to enable services
  - Use `sudo systemctl restart <server name>` to restart services
  - Use `sudo systemctl status <server name>` to check if services are activated
  
     <img width="1461" alt="hello_web_service_status" src="https://user-images.githubusercontent.com/100324443/205280555-5265daf2-3487-4978-ad2f-40dbce6b4861.png">
     <img width="949" alt="caddy_service_status" src="https://user-images.githubusercontent.com/100324443/205280580-52619bb7-2b97-4f43-b5fd-3588a27bb46d.png">
     
## Step 8 - Change file contents in server-02 and test files
- Change owner and group of `index.html` in both `server-01` and `server-02`:
  
- Change file contents:
  - Change `index.html` content in `server-02`
  
     <img width="942" alt="server-02_index_html" src="https://user-images.githubusercontent.com/100324443/205280606-1165a92a-7c5d-4460-9d53-1d18f1a7b645.png">

  - Change `index.js` content in `server-01` and `server-02`
  
      <img width="943" alt="server-01_index_js" src="https://user-images.githubusercontent.com/100324443/205280699-dd9640b0-6360-45fe-8420-745e0eb9e833.png">
      <img width="944" alt="server-02_index_js" src="https://user-images.githubusercontent.com/100324443/205280715-e78c00ef-2da0-4f46-9389-ed8947fd266f.png">

- Test web app and server blocks:
  - Run load-balancer IP address in browser
  - Refresh the browser to check if the page gets random switch
      <img width="911" alt="test_server-01" src="https://user-images.githubusercontent.com/100324443/205280777-799a7ead-55b0-4f6c-803d-f3da76cfc2cd.png">
      <img width="905" alt="test_server-02" src="https://user-images.githubusercontent.com/100324443/205280815-021a77b6-86f7-4d10-933a-f2d4168774bb.png">
      
## Step 9 - Test load-balancer
  - Run load-balancer IP address in browser
  - Refresh the browser to check if the page gets random switch
     <img width="911" alt="test_server-01" src="https://user-images.githubusercontent.com/100324443/205280777-799a7ead-55b0-4f6c-803d-f3da76cfc2cd.png">
     <img width="905" alt="test_server-02" src="https://user-images.githubusercontent.com/100324443/205280815-021a77b6-86f7-4d10-933a-f2d4168774bb.png">
      

 
  - Add `/api` at the end of the IP address to check if api works:
     <img width="658" alt="test_server-01_api" src="https://user-images.githubusercontent.com/100324443/205280928-751dca0a-f81f-4c95-8330-01212312b91f.png">
     <img width="672" alt="test_server-02_api" src="https://user-images.githubusercontent.com/100324443/205280950-51b2d27b-0375-4b7f-a4da-84aaf37b3250.png">




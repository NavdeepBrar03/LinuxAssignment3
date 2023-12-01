# Debian 12 Server Initialization on DigitalOcean

**1. Droplet Setup Process**
- **DigitalOcean Login:** Access your account on the DigitalOcean platform.
- **Droplet Creation:** Click "Create" and select "Droplets".
- **Image Selection:** Under "Choose an image," select "Debian" and opt for version 12.
- **Plan Selection:** Opt for a suitable plan based on your project size.
- **Datacenter Location:** Pick the nearest datacenter to your audience.
- **SSH Key Setup:** Opt for SSH keys for security. Follow DigitalOcean's SSH key setup guide if necessary.
- **Droplet Finalization:** Assign a name, decide on additional features, and initiate the creation.

**2. Server Connection**
- **SSH Connection:** Use SSH to connect to your server:
- Use:
     ```bash
     ssh -i [location] root@your_server_ip
     ```



# Setting Up a User

`sudo`: Executes the command with superuser privileges

## Step One: Create a New User

1. **Create a new user with `sudo` privileges:**
    
    i)**Objective**: To create a new user with a home directory and bash as their default shell.

   - To create a new user, use:
     ```bash
     useradd -ms /bin/bash [username]
     ```
     - `useradd`: Command to create a new user.
     - `-m`: Creates a home directory for the new user.
     - `-s /bin/bash`: Sets `/bin/bash` as the user's login shell.
     - `[username]`: Placeholder for the desired username.
     
       

       
     
   ii) **Objective**: To give the new user administrative privileges.
   - Add this user to the `sudo` group for administrative privileges:
     ```bash
     sudo usermod -aG sudo [username]
     ```
     - `usermod`: Command to modify a user's settings.
     - `-aG`: Appends the user to the specified group.
     - `sudo`: Group name for granting admin privileges.
     - `[username]`: Placeholder for the username created earlier.
       
        

2. **Set a password for the new user:**
    
    **Objective**: To securely set a password for the new user.
   - You will be prompted to set a password during the user creation process.
     ```bash
     passwd [username]
     ```
     - `passwd`: Command to set or change the user's password.
     - `[username]`: Placeholder for the username.
       
      

## Step Two: SSH Setup for New User

1. **Copy `.ssh` directory from root to the new user's home directory:**
    
    **Objective**: To copy SSH configuration from root to the new user.
   - Copy command:
     ```bash
     sudo cp -R /root/.ssh /home/[username]/
     ```
     - `cp`: Command to copy files and directories.
     - `-R`: Recursively copy the entire directory.
     - `/root/.ssh`: Source directory (root's SSH configuration).
     - `/home/[username]/`: Destination directory.
       
      

2. **Change ownership of the copied `.ssh` directory:**
    
    **Objective**: To ensure the new user owns the copied `.ssh` directory.
   - Change ownership with:
     ```bash
     sudo chown -R [username]:[username] /home/[username]/.ssh
     ```
     - `chown`: Command to change file owner and group.
     - `-R`: Apply the changes recursively to all files in the directory.
     - `[username]:[username]`: New owner and group for the files.
     - `/home/[username]/.ssh`: Target directory.
       
      

## Step Three: Test SSH Connection
1. **Log Out of the Current Session:**
    
    **Objective**: To exit from the current user session, preparing to log in as the new user.
   - To log out of your current SSH or user session, simply type:
     ```bash
     exit
     ```
     - `exit`: Command to end the current session.
      


2. **SSH into the server with the new user:**
    
    **Objective**: To test the SSH connection with the new user.
   - Use:
     ```bash
     ssh -i [location] [username]@your_server_ip
     ```
     - `ssh`: Command to initiate a secure shell connection.
     - `-i [location]`: Specifies the private key file location for authentication.
     - `[username]@your_server_ip`: Connects as `[username]` to the server at `your_server_ip`.
       
      

## Step Four: Edit SSH Configuration to Disable Root Login

1. **Edit the SSH configuration file:**
    
    **Objective**: To modify SSH server settings.
   - Open for editing:
     ```bash
     sudo vim /etc/ssh/sshd_config
     ```
     - `vim`: Command to edit files using the Vim editor.
     - `/etc/ssh/sshd_config`: Path to the SSH daemon configuration file.
       
      

2. **Disable root login via SSH:**
   - Change `PermitRootLogin yes` to `PermitRootLogin no`.
   - Uncomment the line if necessary by removing `#`.

3. **Restart the SSH service:**
    
    **Objective**: To apply the new SSH configuration settings.
   - Restart with:
     ```bash
     sudo systemctl restart sshd
     ```
     - `systemctl restart sshd`: Command to restart the SSH daemon.
      

## Step Five: Verifying Root Login Restriction

1. **Log Out of the Current Session:**
    
    **Objective**: To exit from the current user session, preparing to log in as the new user.
   - To log out of your current SSH or user session, simply type:
     ```bash
     exit
     ```
     - `exit`: Command to end the current session.
      

2. **Verify Root Login is Disabled:**
    
    **Objective**: To ensure that root login has been successfully restricted for increased security.
   - Attempt to log in as the root user via SSH:
     ```bash
     ssh -i [location] root@your_server_ip
     ```
     - If root login is correctly disabled, this attempt should fail.
      


# Installing and Configuring Nginx Server

`sudo`: Executes the command with superuser privileges

**This guide outlines the process of installing and configuring the Nginx web server on a Linux-based system in four main steps.**

## Step One: System Preparation

1. **Update the System**
     
     **Objective**: To ensure the latest software and dependencies are available before installing Nginx.
   - Command:
     ```bash
     sudo apt update
     ```
      - `apt update`: Updates the package list for the system.
     

2. **Install Nginx**
    
    **Objective**: To install Nginx as the web server on the system.
   - Command:
     ```bash
     sudo apt install nginx
     ```
      - `apt install nginx`: Installs the Nginx package.
      

## Step Two: Configure Nginx

1. **Enable and Start Nginx**
    
    **Objective**: To ensure Nginx runs automatically upon system boot and starts running immediately.
   - Commands:
     ```bash
     sudo systemctl enable nginx
     sudo systemctl start nginx
     ```
      - `sudo`: Executes the commands with superuser privileges.
      - `systemctl enable nginx`: Enables Nginx to start at boot.
      - `systemctl start nginx`: Starts the Nginx service.
      

2. **Navigate to Web Root**
    
    **Objective**: To access the default directory where web files are stored.
   - Command:
     ```bash
     cd /var/www
     ```
      - `cd /var/www`: Changes the current directory to Nginx's web root.
      

3. **Create a New Directory**
    
    **Objective**: To create a specific directory for your website's files.
   - Command:
     ```bash
     sudo mkdir [dir]
     ```
      - `mkdir [dir]`: Creates a new directory.
      - Replace `[dir]` with your directory name.
      

## Step Three: Add Web Content

1. **Create an HTML File**
    
    **Objective**: To add custom web content, such as a homepage.
   Inside the directory you created in the earlier step, create an HTML file using the following command. This HTML file will display a simple "Hello, World" message.

   - Command:
     ```bash
     sudo vim index.html
     ```
      - `vim index.html`: Opens the Vim editor to create or edit an `index.html` file.
      

   **HTML Code to Copy:**
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>2420</title>
       <style>
           body {
               display: flex;
               align-items: center;
               justify-content: center;
               height: 100vh;
               margin: 0;
           }
           h1 {
               text-align: center;
           }
       </style>
   </head>
   <body>
       <h1>Hello, World</h1>
   </body>
   </html>
     
## Step Four: Server Block Configuration

1. **Creating a Custom Server Block**
    
    **Objective**: To configure a server block for hosting websites.
   - Use this command to create and edit a new server block configuration file. This configuration tells Nginx how to handle incoming requests and serve your website.

   - Command:
     ```bash
     sudo vim /etc/nginx/sites-available/[my-server]
     ```
      - Replace `[my-server]` with your file name.
      - `vim /etc/nginx/sites-available/[my-server]`: Opens the Vim editor to create or edit the server block configuration file.
      
   **Server Block Sample Code:**
   Replace `[your dir]` with the directory name you created in `/var/www`.
   ```nginx
   server {
       listen 80 default_server;
       listen [::]:80 default_server;
       
       root /var/www/[your dir];
       
       index index.html index.htm index.nginx-debian.html;
       
       server_name _;
       
       location / {
           # First attempt to serve request as file, then
           # as directory, then fall back to displaying a 404.
           try_files $uri $uri/ =404;
       }
   }


2. **Enable the Server Block**
    
    **Objective**: To enable the new server block configuration and disable the default.
   - Commands:
     ```bash
     sudo ln -s /etc/nginx/sites-available/[my-server] /etc/nginx/sites-enabled/
     sudo unlink /etc/nginx/sites-enabled/default
     ```
      - `sudo`: Executes the commands with superuser privileges.
      - `ln -s`: Creates a symbolic link.
      - `unlink`: Removes the existing symbolic link.
      
3. **Reload or Restart Nginx**
    
    **Objective**: To apply the changes made to the Nginx configuration.
   - Commands:
     ```bash
     sudo systemctl reload nginx
     sudo systemctl restart nginx
     ```
      - `sudo`: Executes the commands with superuser privileges.
      - `systemctl reload nginx`: Reloads Nginx to apply new configurations.
      - `systemctl restart nginx`: Restarts the Nginx service.
      


4. **Verify Nginx Configuration**
     
     **Objective**: To ensure that there are no syntax errors in your Nginx configuration files.
   - Before restarting the Nginx service, it's a good practice to check if the configuration file syntax is correct.
   - Command:
     ```bash
     sudo nginx -t
     ```
      - `nginx -t`: Tests the configuration files for syntax errors.
     



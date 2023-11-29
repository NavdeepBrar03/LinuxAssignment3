# Setting Up a User
## Step One: Create a New User

1. **Create a new user with `sudo` privileges:**
   - To create a new user, use:
     ```bash
     useradd -ms /bin/bash [username]
     ```
     - `useradd`: Command to create a new user.
     - `-m`: Creates a home directory for the new user.
     - `-s /bin/bash`: Sets `/bin/bash` as the user's login shell.
     - `[username]`: Placeholder for the desired username.
     - **Purpose**: To create a new user with a home directory and bash as their default shell.
   - Add this user to the `sudo` group for administrative privileges:
     ```bash
     sudo usermod -aG sudo [username]
     ```
     - `sudo`: Executes the command with superuser privileges.
     - `usermod`: Command to modify a user's settings.
     - `-aG`: Appends the user to the specified group.
     - `sudo`: Group name for granting admin privileges.
     - `[username]`: Placeholder for the username created earlier.
     - **Purpose**: To give the new user administrative privileges.

2. **Set a password for the new user:**
   - You will be prompted to set a password during the user creation process.
     ```bash
     passwd [username]
     ```
     - `passwd`: Command to set or change the user's password.
     - `[username]`: Placeholder for the username.
     - **Purpose**: To securely set a password for the new user.

## Step Two: SSH Setup for New User

1. **Copy `.ssh` directory from root to the new user's home directory:**
   - Copy command:
     ```bash
     sudo cp -R /root/.ssh /home/[username]/
     ```
     - `sudo`: Executes the command with superuser privileges.
     - `cp`: Command to copy files and directories.
     - `-R`: Recursively copy the entire directory.
     - `/root/.ssh`: Source directory (root's SSH configuration).
     - `/home/[username]/`: Destination directory.
     - **Purpose**: To copy SSH configuration from root to the new user.

2. **Change ownership of the copied `.ssh` directory:**
   - Change ownership with:
     ```bash
     sudo chown -R [username]:[username] /home/[username]/.ssh
     ```
     - `sudo`: Executes the command with superuser privileges.
     - `chown`: Command to change file owner and group.
     - `-R`: Apply the changes recursively to all files in the directory.
     - `[username]:[username]`: New owner and group for the files.
     - `/home/[username]/.ssh`: Target directory.
     - **Purpose**: To ensure the new user owns the copied `.ssh` directory.

## Step Three: Test SSH Connection

1. **SSH into the server with the new user:**
   - Use:
     ```bash
     ssh -i [location] [username]@your_server_ip
     ```
     - `ssh`: Command to initiate a secure shell connection.
     - `-i [location]`: Specifies the private key file location for authentication.
     - `[username]@your_server_ip`: Connects as `[username]` to the server at `your_server_ip`.
     - **Purpose**: To test the SSH connection with the new user.

## Step Four: Edit SSH Configuration to Disable Root Login

1. **Edit the SSH configuration file:**
   - Open for editing:
     ```bash
     sudo vim /etc/ssh/sshd_config
     ```
     - `sudo`: Executes the command with superuser privileges.
     - `vim`: Command to edit files using the Vim editor.
     - `/etc/ssh/sshd_config`: Path to the SSH daemon configuration file.
     - **Purpose**: To modify SSH server settings.

2. **Disable root login via SSH:**
   - Change `PermitRootLogin yes` to `PermitRootLogin no`.
   - Uncomment the line if necessary by removing `#`.

3. **Restart the SSH service:**
   - Restart with:
     ```bash
     sudo systemctl restart sshd
     ```
     - `sudo`: Executes the command with superuser privileges.
     - `systemctl restart sshd`: Command to restart the SSH daemon.
     - **Purpose**: To apply the new SSH configuration settings.



# Installing and Configuring Nginx Server

**This guide outlines the process of installing and configuring the Nginx web server on a Linux-based system in four main steps.**

## Step One: System Preparation

1. **Update the System**
   - Command:
     ```bash
     sudo apt update
     ```
   - `sudo`: Executes the command with superuser privileges.
   - `apt update`: Updates the package list for the system.
   - **Purpose**: To ensure the latest software and dependencies are available before installing Nginx.

2. **Install Nginx**
   - Command:
     ```bash
     sudo apt install nginx
     ```
   - `sudo`: Executes the command with superuser privileges.
   - `apt install nginx`: Installs the Nginx package.
   - **Purpose**: To install Nginx as the web server on the system.

## Step Two: Configure Nginx

1. **Enable and Start Nginx**
   - Commands:
     ```bash
     sudo systemctl enable nginx
     sudo systemctl start nginx
     ```
   - `sudo`: Executes the commands with superuser privileges.
   - `systemctl enable nginx`: Enables Nginx to start at boot.
   - `systemctl start nginx`: Starts the Nginx service.
   - **Purpose**: To ensure Nginx runs automatically upon system boot and starts running immediately.

2. **Navigate to Web Root**
   - Command:
     ```bash
     cd /var/www
     ```
   - `cd /var/www`: Changes the current directory to Nginx's web root.
   - **Purpose**: To access the default directory where web files are stored.

3. **Create a New Directory**
   - Command:
     ```bash
     sudo mkdir [dir]
     ```
   - `sudo`: Executes the command with superuser privileges.
   - `mkdir [dir]`: Creates a new directory.
   - Replace `[dir]` with your directory name.
   - **Purpose**: To create a specific directory for your website's files.

## Step Three: Add Web Content

1. **Create an HTML File**
   - Command:
     ```bash
     vim index.html
     ```
   - `vim index.html`: Opens the Vim editor to create or edit an `index.html` file.
   - **Purpose**: To add custom web content, such as a homepage.

## Step Four: Server Block Configuration

1. **Creating a Custom Server Block**
   - Command:
     ```bash
     vim /etc/nginx/sites-available/my-server
     ```
   - `vim /etc/nginx/sites-available/my-server`: Opens the Vim editor to create or edit the server block configuration file.
   - **Purpose**: To configure a server block for hosting websites.

2. **Enable the Server Block**
   - Commands:
     ```bash
     sudo ln -s /etc/nginx/sites-available/my-server /etc/nginx/sites-enabled/
     sudo unlink /etc/nginx/sites-enabled/default
     ```
   - `sudo`: Executes the commands with superuser privileges.
   - `ln -s`: Creates a symbolic link.
   - `unlink`: Removes the existing symbolic link.
   - **Purpose**: To enable the new server block configuration and disable the default.

3. **Reload or Restart Nginx**
   - Commands:
     ```bash
     sudo systemctl reload nginx
     sudo systemctl restart nginx
     ```
   - `sudo`: Executes the commands with superuser privileges.
   - `systemctl reload nginx`: Reloads Nginx to apply new configurations.
   - `systemctl restart nginx`: Restarts the Nginx service.
   - **Purpose**: To apply the changes made to the Nginx configuration.




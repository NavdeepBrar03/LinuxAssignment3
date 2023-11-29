## Step One: Create a New User

1. **Create a new user with `sudo` privileges:**
   - To create a new user, use:
     ```bash
       useradd -ms /bin/bash [user-name]
     ```
     (replace `[username]` with your desired username).
   - Add this user to the `sudo` group for administrative privileges: ```bash sudo usermod -aG sudo [username]```.

2. **Set a password for the new user:**
   - You will be prompted to set a password during the user creation process.
   - ```bash passwd <user-name>```


## Step Two: SSH Setup for New User

1. **Copy `.ssh` directory from root to the new user's home directory:**
   - Copy command: ```bash sudo cp -R /root/.ssh /home/[username]/```.

2. **Change ownership of the copied `.ssh` directory:**
   - Change ownership with: ```bash sudo chown -R [username]:[username] /home/[username]/.ssh```.

## Step Three: Test SSH Connection

1. **SSH into the server with the new user:**
   - Use: ```bash ssh [username]@your_server_ip``` (replace `[username]` and `your_server_ip` appropriately).

## Step Four: Edit SSH Configuration to Disable Root Login

1. **Edit the SSH configuration file:**
   - Open for editing: ```sudo vim /etc/ssh/sshd_config```.

2. **Disable root login via SSH:**
   - Change `PermitRootLogin yes` to `PermitRootLogin no`.
   - Uncomment the line if necessary by removing `#`.

3. **Restart the SSH service:**
   - Restart with: ```bash sudo systemctl restart sshd```.

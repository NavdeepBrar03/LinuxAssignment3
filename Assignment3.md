## Step One: Create a New User

1. **Create a new user with `sudo` privileges:**
   - To create a new user, use: `sudo adduser [username]` (replace `[username]` with your desired username).
   - Add this user to the `sudo` group for administrative privileges: `sudo usermod -aG sudo [username]`.

2. **Set a password for the new user:**
   - You will be prompted to set a password during the user creation process.

3. **Set bash as the login shell:**
   - Usually default. To verify or change: `sudo chsh -s /bin/bash [username]`.

## Step Two: SSH Setup for New User

1. **Copy `.ssh` directory from root to the new user's home directory:**
   - Copy command: `sudo cp -R /root/.ssh /home/[username]/`.

2. **Change ownership of the copied `.ssh` directory:**
   - Change ownership with: `sudo chown -R [username]:[username] /home/[username]/.ssh`.

## Step Three: Test SSH Connection

1. **SSH into the server with the new user:**
   - Use: `ssh [username]@your_server_ip` (replace `[username]` and `your_server_ip` appropriately).

## Step Four: Edit SSH Configuration to Disable Root Login

1. **Edit the SSH configuration file:**
   - Open for editing: `sudo nano /etc/ssh/sshd_config`.

2. **Disable root login via SSH:**
   - Change `PermitRootLogin yes` to `PermitRootLogin no`.
   - Uncomment the line if necessary by removing `#`.

3. **Restart the SSH service:**
   - Restart with: `sudo systemctl restart sshd`.

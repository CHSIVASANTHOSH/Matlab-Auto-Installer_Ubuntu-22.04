# Matlab-Auto-Installer_Ubuntu-22.04
Automation script where It automatically downloads and installs the Matlab .iso file and removes the unmounted file
# If you are working in windows and want to install Matlab in the Ubuntu 22.04 system then follow the below instructions
- Open power shell and paste the command  scp C:/Users/user_name/Downloads/network (1).lic remote_username@remote_user_ip:/home/remote_user_name/Downloads
- It will be forwarded to the remote system you can check by pressing
- ```cd Downloads```
     ```ls ```
- Create a install_matlab.sh  file by opening terminal pressing ```ctl+alt+t``` and in terminal type  ```sudo nano install_matlab.sh``` and paste
```bash
#!/bin/bash
# Define variables
URL="http://swrepo.iitkgp.ac.in/Matlab2023B/R2023b_Linux.iso" # Add any URL you have to download it 
OUTPUT_PATH="/tmp/R2023b_Linux.iso"
MOUNT_POINT="/mnt/matlab_iso"
INSTALL_KEY="xxxxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxx-xxxxxxxx" # Installation key
LICENSE_FILE_PATH="/path/to/license_file.lic" # Replace with the actual path to your license file

# Create a temporary directory for the download
mkdir -p /tmp

# Download the ISO
wget -O $OUTPUT_PATH $URL

# Create mount point
sudo mkdir -p $MOUNT_POINT

# Mount the ISO
sudo mount -o loop $OUTPUT_PATH $MOUNT_POINT

# Create installer input file
INSTALLER_INPUT="/tmp/installer_input.txt"
cat <<EOT > $INSTALLER_INPUT
destinationFolder=/usr/local/MATLAB/R2023b
fileInstallationKey=$INSTALL_KEY
licensePath=$LICENSE_FILE_PATH
agreeToLicense=yes
mode=silent
EOT

# Run the installer
sudo $MOUNT_POINT/install -inputFile $INSTALLER_INPUT

# Unmount the ISO
sudo umount $MOUNT_POINT

# Clean up
rm -rf /tmp/R2023b_Linux.iso /tmp/installer_input.txt
sudo rmdir $MOUNT_POINT

echo "MATLAB installation completed successfully." ```


2. Replace Placeholders
Replace YOUR_FILE_INSTALLATION_KEY with your actual MATLAB file installation key.
Replace /home/your_username/Downloads/network (1).lic with the path to your MATLAB license file.
3. Save and Exit nano
Press Ctrl + O to save the file.
Press Enter to confirm the filename.
Press Ctrl + X to exit nano.
4. Make the Script Executable
Run the following command to make the script executable:

bash
Copy code
chmod +x install_matlab.sh
5. Run the Script
Execute the script using sudo:

bash
Copy code
sudo ./install_matlab.sh
Transferring the License File
If your license file is on a Windows machine, use scp to transfer it to your Ubuntu machine.

Open PowerShell on Windows:

powershell
Copy code
scp "C:\Users\your_username\Downloads\network (1).lic" your_ubuntu_username@your_ubuntu_ip:/home/your_ubuntu_username/Downloads/
Enter the SSH password when prompted.

Verification
After running the script, you can verify the MATLAB installation by launching MATLAB:

bash
Copy code
/usr/local/MATLAB/R2023b/bin/matlab

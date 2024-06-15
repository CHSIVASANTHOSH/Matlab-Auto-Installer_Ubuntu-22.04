# Matlab-Auto-Installer_Ubuntu-22.04

Automation script to automatically download and install the MATLAB .iso file and remove the unmounted file.

## If you are working on Windows and want to install MATLAB on an Ubuntu 22.04 system, follow these instructions:

### 1. Transfer the License File

1. Open PowerShell and run the following command to transfer the license file to your Ubuntu system:

    ```powershell
    scp C:/Users/user_name/Downloads/network (1).lic remote_username@remote_user_ip:/home/remote_user_name/Downloads
    ```

2. Verify the transfer on the remote system by navigating to the Downloads directory:

    ```bash
    cd Downloads
    ls
    ```

### 2. Create the Installation Script

1. Open a terminal on your Ubuntu system by pressing `Ctrl+Alt+T`.
2. Create a new script file:

    ```bash
    sudo nano install_matlab.sh
    ```

3. Copy and paste the following script into the `nano` editor:

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

    echo "MATLAB installation completed successfully."
    ```

### 3. Replace Placeholders

- Replace `xxxxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxx-xxxxxxxx` with your actual MATLAB file installation key.
- Replace `/path/to/license_file.lic` with the path to your MATLAB license file.

### 4. Save and Exit `nano`

- Press `Ctrl + O` to save the file.
- Press `Enter` to confirm the filename.
- Press `Ctrl + X` to exit `nano`.

### 5. Make the Script Executable

Run the following command to make the script executable:

```bash
chmod +x install_matlab.sh
```
### 6. Run the Script
Execute the script using sudo:

Run the following command to make the script executable:

```bash
sudo ./install_matlab.sh
```
### Transferring the License File
Execute the script using sudo:
If your license file is on a Windows machine, use scp to transfer it to your Ubuntu machine.
-Open PowerShell on Windows:
```bash
scp "C:\Users\your_username\Downloads\network (1).lic" your_ubuntu_username@your_ubuntu_ip:/home/your_ubuntu_username/Downloads/
```
-Enter the SSH password when prompted.
#Verification
After running the script, you can verify the MATLAB installation by launching MATLAB:

```bash
Copy code
/usr/local/MATLAB/R2023b/bin/matlab
```

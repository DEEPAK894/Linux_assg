# Linux_assignment

### 1)  Configure smtp in localhost.<br>
Ans:-<br>
Step 1: Install Postfix  on our Linux machine using the following command:<br>
               ***sudo apt install postfix***<br>
              
  Step 2: Configure Postfix.<br>
        1. During the configuration, choose "Internet Site" and proceed.<br>
        2. If needed, we can customize our mail settings. <br>
        3. The main configuration file for Postfix is usually located at /etc/postfix/main.cf. <br>
        4. We can edit this file using a text editor like nano  <br>
           ***sudo nano /etc/postfix/main.cf*** <br>
        In the file, modify the line  inet_interfaces = all to inet_interfaces = loopback-only <br>
        
  Step 3: Install mailutils.<br>
           ***sudo apt install mailutils***  <br>
  Step 4: Test the Configuration.<br>
        We can now test our Postfix configuration by sending a test email using the following command.<br>
        ***echo "This is the body of the email" | mail -s "Subject Line" email@mydomain.com*** <br>
  
### 2)  Create a user in your localhost, which should not be able to execute the sudo command. <br>
Ans:-
     - To create a new user, use the following command<br>
           ***sudo useradd newuser*** <br><br>
     - By default, on most Unix-based systems, regular users are not in the sudo group, so they do not have sudo privileges.
<br>

### 3) Configure your system in such a way that when a user type and executes a describe command from anywhere of the system    it must list all the files and folders of the user's current directory. <br>
 Ans:-<br>
 Step 1: Create a Shell Script.<br>
      - Create a new file, describe.sh in the directory /usr/local/bin, and add the following lines to it:<br>
```
         #!/bin/bash
            ls -a
```
<br>
      - This script simply runs the ls -a command, which lists all files and folders in the current directory,
   including hidden files<br>
 Step 2: Make the Script Executable:<br>
      - Make the script executable using the following command:<br>
         ***chmod a+x describe.sh***<br>
Step 3: Test the Configuration by typing the describe in the terminal<br><br>

### 4) Users can put a compressed file at any path of the linux file system. The name of the file will be research and the extension will be of compression type, example for gzip type extension will be .gz. You have to find the file and check the compression type and uncompress it.<br>
Ans:-
   Step 1: Create a New Shell Script with file name as uncompress_research.sh using nano command and add the below code into the file.
 ```
 #!/bin/bash

# Find the compressed file named "research" with any compression extension
compressed_file=$(find / -type f -name "research.*" 2>/dev/null)

if [ -z "$compressed_file" ]; then
    echo "No compressed file named 'research' found."
    exit 1
fi

# Get the file extension
file_extension="${compressed_file##*.}"

# Determine the compression type and uncompress the file
case "$file_extension" in
    "gz")
        # Gzip compression
        uncompressed_file="${compressed_file%.gz}"
        gzip -d "$compressed_file"
        echo "File '$compressed_file' has been uncompressed using gzip."
        ;;
    "zip")
        # Zip compression
        uncompressed_file="${compressed_file%.zip}"
        unzip "$compressed_file" -d "$(dirname $compressed_file)"
        echo "File '$compressed_file' has been uncompressed using zip."
        ;;
    *)
        # Unsupported compression type
        echo "Unsupported compression type: .$file_extension"
        exit 1
        ;;
esac

echo "Uncompressed file: $uncompressed_file"
```
 <br>Step 2: Run the Script<br>
      ***./uncompress_research.sh***<br>
       - The script will start searching for a compressed file named "research" with a specific extension (e.g., .gz or .zip) in our Linux filesystem. If it finds the file, it will uncompress it accordingly and provide you with the output.<br><br>
### 5) Configure your system in such a way that any user of your system creates a file then there should not be permission to do any activity in that file.<br>
Ans:-
  Step 1: Use umask command for specific session<br>
        ***umask 0777***<br>
  Step 2: Apply umask to root<br>
        ***sudo nano bash.bashrc  or sudo nano /etc/login.defs*** <br>
        - Scroll down and change umask 022 to 0777<br><br>

### 6) Create a service with the name showtime , after starting the service, every minute it should print the current time in a file in the user home directory.<br>   Ex:-sudo service showtime start -> It should start writing in file.<br> sudo service showtime stop -> It should stop writing in file. <br>  sudo service showtime status -> It should show status.<br>
 Ans:-<br>
Step 1: Create the Shell Script hat will write the current time to a file in the user's home directory.
               - Name the script as showtime.sh and write below content into the file:
```
 #!/bin/bash
 FILE_PATH="/home/sigmoid/showtime_output.txt"
 while true; do
 echo $(date) >> "$FILE_PATH"
 sleep 60  
 done
```
Step 2: Make the script executable<br>
           ***chmod +x showtime.sh*** <br>
 Step 3: Create a systemd service file named showtime.service in the ***/etc/systemd/system/*** directory and write the below code into that file.
  ```  
     [Unit]
     Description=Showtime Service
     After=network.target
     [Service]
     ExecStart=/home/sigmoid/showtime.sh
     Restart=always
     User=sigmoid
     Group=sigmoid
     [Install]
     WantedBy=multi-user.target
  ```
  
  The script will start searching for a compressed file named "research" with a specific extension (e.g., .gz or .zip) in your Linux filesystem. If it finds the file, it will uncompress it accordingly and provide you with the output.

Step 4:  Reload Systemd:<br>
                After making changes to the service unit file, you need to reload the systemd manager configuration.                    Run the following command to reload systemd:<br>
                 ***sudo systemctl daemon-reload***<br>
 Step 5: Manage the Service<br>
               - Command to start the service : ***sudo systemctl start showtime***<br>
               - Command to stop the service  : ***sudo systemctl stop showtime***<br>
               - Command to check the service status : ***sudo systemctl status showtime***<br>
             * To check the output of the showtime.sh script, which continuously appends the current time to a file (showtime_output.txt).<br>
             * If we want to continuously monitor the output file in real-time, you can use the tail command with the -f (follow) option: <br>
            ***tail -f ~/showtime_output.txt***

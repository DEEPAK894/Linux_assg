# Linux_assignment

1)  Configure smtp in localhost.
Ans:-
 Step 1: Install Postfix  on our Linux machine using the following command:
          command = { sudo apt install postfix }
 Step 2: Configure Postfix.
      - During the configuration, choose "Internet Site" and proceed.
        If needed, we can customize our mail settings.
    The main configuration file for Postfix is usually located at /etc/postfix/main.cf.
      - We can edit this file using a text editor like nano 
           command = { sudo nano /etc/postfix/main.cf }
      - In the file, modify the line  inet_interfaces = all to inet_interfaces = loopback-only  
  Step 3: Install mailutils.
           command = { sudo apt install mailutils }      
  Step 4: Test the Configuration.
      - We can now test our Postfix configuration by sending a test email using the following command.
        command = { echo "This is the body of the email" | mail -s "Subject Line" email@mydomain.com }

        
2)  Create a user in your localhost, which should not be able to execute the sudo command.
Ans:-
      - To create a new user, use the following command
         command = { sudo useradd newuser }
By default, on most Unix-based systems, regular users are not in the sudo group, so they do not have sudo privileges.


3) Configure your system in such a way that when a user type and executes a describe command from anywhere of the system    it must list all the files and folders of the user's current directory.
Ans:-
 Step 1: Create a Shell Script.
      - Create a new file, describe.sh in the directory /usr/local/bin, and add the following lines to it:
            #!/bin/bash
            ls -a
   This script simply runs the ls -a command, which lists all files and folders in the current directory,
   including hidden files
 Step 2: Make the Script Executable:
      - Make the script executable using the following command:
         command = { chmod a+x describe.sh }
 Step 3: Test the Configuration by typing the describe in the termianl

4) Users can put a compressed file at any path of the linux file system. The name of the file will be research
   and the      extension will be of compression type, example for gzip type extension will be .gz.
   You have to find the file and check the compression type and uncompress it.
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












 5)  Configure your system in such a way that any user of your system creates a file then there should not be permission      to do any activity in that file.
Ans:-
  Step 1: Use umask command for specific session
         command { umask 0777 }
  Step 2: Apply umask to root
         command { sudo nano bash.bashrc  or sudo nano /etc/login.defs}
        - Scroll down and change umask 022 to 0777

 6) Create a service with the name showtime , after starting the service, every minute it should print the current time      in a file in the user home directory.    
    Ex:-
       sudo service showtime start   -> It should start writing in file.
       sudo service showtime stop   -> It should stop writing in file.
       sudo service showtime status -> It should show status.
    Ans:-
       Step 1: Create the Shell Script hat will write the current time to a file in the user's home directory.
               - Name the script as showtime.sh and write below content into the file:
               #!/bin/bash
               FILE_PATH="/home/sigmoid/showtime_output.txt"
               while true; do
               echo $(date) >> "$FILE_PATH"
               sleep 60  
               done
       Step 2: Make the script executable
            command = { chmod +x showtime.sh }
       Step 3: Create a systemd service file named showtime.service in the /etc/systemd/system/ directory and write the
               below code into that file.
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
       Step 4:  Reload Systemd:
                After making changes to the service unit file, you need to reload the systemd manager configuration.                    Run the following command to reload systemd:
                  command = { sudo systemctl daemon-reload }
       Step 5: Manage the Service
               -Command to start the service : sudo systemctl start showtime
               -Command to stop the service  : sudo systemctl stop showtime
               -Command to check the service status : sudo systemctl status showtime
          * To check the output of the showtime.sh script, which continuously appends the current time to a file (showtime_output.txt).
          * If we want to continuously monitor the output file in real-time, you can use the tail command with the -f (follow) option: command = { tail -f ~/showtime_output.txt }

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

3) Configure your system in such a way that when a user type and executes a describe command from anywhere of the system it must list all the files and folders of the user's current directory.

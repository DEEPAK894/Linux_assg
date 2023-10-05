# Linux_assg

1)  Configure smtp in localhost.
Ans:-
 Step 1: We have to install postfix first in our linux machine.
          command = { sudo apt install postfix }
 Step 2: Configure Postfix.
      - select internet site then enter tab and enter.
      - enter custom mail
    The main configuration file for Postfix is usually located at /etc/postfix/main.cf.
      - We can edit this file using a text editor like nano 
           command = { sudo nano /etc/postfix/main.cf }
      - In the file change the line [inet_interfaces = ali to inet_intterfaces = loopback-only  
  Step 3: Install mailutils.
           command = { sudo apt install mailutils }      
  Step 4: Test the Configuration.
      - we can now test your Postfix configuration by sending a test email using the mail command

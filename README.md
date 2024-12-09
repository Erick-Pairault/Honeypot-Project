# Honeypot Project
These are the installation steps and results of my honeypot project utilizing T-Pot and AWS (Windows 11).

# EC2 Set-Up on AWS
1. Create an AWS Account.
2. Navigate to the EC2 Dashboard.
3. Select "Launch instance"
4. **Name and tags:** name the instance anything (e.g. Honeypot).
5. **Application and OS Images:** select from "Browse more AMIs" on the right. Under the search bard select "AWS Marketplace AMIs" and search for Debian. Then select Debian 11 (note: you will have to subscribe; it is very inexpensive).
6. **Instance type:** from the drop-down menu select t3.large (again not free, but not very expensive).
7. **Key pair:** Create a new key pair, name the key pair anything (e.g. Honeykey) and leave the type as RSA and .pem. Save to a location you can easily access (e.g. Documents).
8. **Network settings:** leave the VPC, Subnet, and Auto-assign public IP default. Under Firewall (security groups) select create new group. Under for "Allow SSH traffic from" select "My IP".
9. **Configure storage:** Update the storage to 40.
10. Press launch instance.

# SSH into Machine
11. Within the EC2 Instances, you will see your instance you have just created. Select the instance and click connect on the top bar. Navigate to SSH client and copy the example ssh command at the bottom.
12. Open the command prompt and cd into the directory containing your key (e.g. Documents).
13. Paste the ssh command you copied and press enter. Type yes when prompted if you are sure you want to connect.
 
# Installing T-Pot on the Machine
14. Now that you are ssh into your machine, run the command ``sudo apt-get update``   
15. Then ensure git is set up with the command ``sudo apt install git``
16. Clone the t-pot repository using the command ``git clone https://github.com/telekom-security/tpotce``
17. Navigate to the T-Pot folder with the install using the command ``cd tpotce``
18. Run the install using the command ``./install.sh``
19. Throughout the installation when prompted which version of T-Pot to install select the standard with the command ``h``
20. Throughout the installation when prompted to make a username/passed, make sure that you will remember them (they will be used for the UI window).
21. Once the installation is complete you'll want to reboot the system using the command ``sudo reboot`` which will complete the set-up process.

# Configuring the Server
22. After the installation, return to the EC2 instances dashboard in AWS.
23. Select from the menu options on the left under "Network & Security" the option **Security Groups** and select your security group for the instance you have just created (should have Debian 11 in the name).
24. Under **Inbound Rules** select "Edit inbound rules".
25. You can delete the rule created during the launch instance (step 8).
26. Add a rule **Type:** Custom TCP, **Port Range:** 0-64000, **Source:** Custom with the range 0.0.0.0/0.
27. Add a rule **Type:** Custom TCP, **Port Range:** 64295-64297, **Source:** MY IP.

# Viewing the Dashboard
I used the Microsoft Edge browser but on any browser you will want to edit the settings to allow you to access the T-Pot dashboard. The link to connect will be https://"ec2-machine-ip-address":64297 where "ec2-machine-ip-address" is the IP address of the instance found under **Public IPv4 address**. For Microsoft Edge, after getting the security warning, click on "Advanced" and then "continue anyways". This is where you will input the username and password created during the T-Pot installation process (step 20).

# Resources
- [telekom-security/tpotce: 🍯 T-Pot - The All In One Multi Honeypot Platform 🐝](https://github.com/telekom-security/tpotce)
- https://aws.amazon.com/free/?trk=7d475c59-1af1-47f8-b4c7-88482cbd0e73&sc_channel=ps&s_kwcid=AL!4422!10!71193612981017!71194138969792&ef_id=69c8fd49528014e9d35c9a1e657a443a:G:s&msclkid=69c8fd49528014e9d35c9a1e657a443a&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all

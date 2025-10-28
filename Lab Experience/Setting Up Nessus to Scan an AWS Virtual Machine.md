# Setting Up Nessus to Scan an AWS Virtual Machine
**Category:** CySA+  
**Date:** 26/10/2025
**Author:** Nnamso Mkpong

---

## Objective
- Install Nessus (Essentials), 
- Build a legal target server in AWS, 
- Run a vulnerability scan from your local Nessus VM (VMware Workstation 17 Pro) and save the report.

---

## Tools
- Nessus
- Ubuntu
- AWS account
- VMware Workstation 17 Pro

---
## Step 1
**Part 1: Download Nessus Essentials**
- Navigate to Tenable Nessus Essentials https://www.tenable.com/products/nessus/nessus-essentials
- Complete the registration form with your details
- Check your email for the activation code from Tenable
- Click on the Download button on the Activation email.
- Click on the View downloads on the Tenable Nessus section.
- Download Appropriate Version 

---

**Part 2: Install Nessus and verify**
- In your Ubuntu Terminal, type the command: sudo dpkg -i Nessus-*.deb
- Start Nessus service by typing in the command:- sudo systemctl start nessusd
- Check if it's running by typing in the command: sudo systemctl status nessusd
- Open web browser and navigate to: https://localhost:8834
- Complete first-time setup wizard: 
   Create admin account credentials.
   Enter activation code from email.
   Wait for plugin downloads to complete Wait for plugin downloads to complete. (this can take 10–20 minutes depending on network speed).

- Successful login to Nessus web interface 

---

### STEP 2
**Part 1: Creating AWS account**
- Navigate to Aws https://signin.aws.amazon.com/signup?request_type=register
- Fill in your email address and check your inbox for code.
- Fill in your verification code.
- Enter your password..
- Choose a plan and fill in your payment card details.
- When successful, you should receive a Welcome email. 


---

### STEP 3 Create the EC2 target AWS
- Before you begin to create your EC2 Target, you need to know what your VM public IP as this will use as IP in your EC2 Security Group to allow only your Nessus VM to reach the instance.
	To check your public IP in ubuntu, Type this command: curl -s https://checkip.amazonaws.com
- Save the result for later.

- In the AWS Console, go to the EC2 service and click the Launch Instance button.
- Create a name
- Choose AMI: Ubuntu Server 22.04 LTS (official)
- Choose Instance type: t2.micro (free-tier)
- On choosing a Key pair: Create key pair and download the .pem file to VM and keep it safe
- On Networking Settings: 
	Choose create security group
	Allow SSH Traffic
	Replace the source IP with the public IP of your Nessus VM (for example, YOUR_VM_PUBLIC_IP/32)
	Allow HTTP Traffic from the Internet
	
- Launch the instance and wait for state running.
- In the Instance summary note the IpV4 Public address as that your target ip (EC2)

---

## Run a Nessus Scan
- Open Nessus in your VM: https://127.0.0.1:8834/#/
- Log in to Nessus. 
- From the main navigation, access the Scans section, create a new scan, and choose the Basic Network Scan template
- Name: AWS Target, Targets: 16.171.16.0
- Click Launch, then monitor progress in the ‘Scans’ tab. Once complete, export the report as HTML or PDF for documentation.

## Conclusion
The AWS EC2 instance demonstrated good basic security hygiene, exposing only essential services.

**Overall Security Posture:** Satisfactory and no immediate critical threats identified.

## Lessons Learned
- Ensured safe, permission-based scanning practices.
- Understood how network configuration affects scan visibility.
- Learned to interpret Nessus scan results and prioritize remediation.

## References
- Tenable Nessus Essentials: https://www.tenable.com/products/nessus/nessus-essentials  
- AWS EC2 Documentation: https://docs.aws.amazon.com/ec2  
- Nessus User Guide: https://docs.tenable.com/Nessus
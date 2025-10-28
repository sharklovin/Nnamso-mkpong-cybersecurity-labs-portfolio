# Setting Up Nessus to Scan an AWS Virtual Machine
**Category:** CySA+  
**Date:** 26/10/2025
**Author:** Nnamso Mkpong

---

## Objective
- Install Nessus (Essentials). 
- Build a legal target server in AWS.
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

<img width="1362" height="617" alt="Tenable" src="https://github.com/user-attachments/assets/0eb67e24-9640-4aaf-a958-126d66a941c9" />

- Complete the registration form with your details
- Check your email for the activation code from Tenable
- Click on the Download button on the Activation email.
  <img width="1103" height="555" alt="tenable email" src="https://github.com/user-attachments/assets/ae6ef5bc-65c2-413f-ad10-831bf80bd1bd" />

- Click on the View downloads on the Tenable Nessus section.
 <img width="1285" height="511" alt="click on the activation email" src="https://github.com/user-attachments/assets/d4a41815-878c-4e3a-9ade-24ff985bf9b6" />

- Download Appropriate Version
  <img width="1071" height="485" alt="Download Nessus" src="https://github.com/user-attachments/assets/beb31355-4fd1-4ebb-963d-848ac9d19372" />


---

**Part 2: Install Nessus and verify**
- In your Ubuntu Terminal, type the command: `sudo dpkg -i Nessus-*.deb`
- Start Nessus service by typing in the command:- `sudo systemctl start nessusd`
- Check if it's running by typing in the command: `sudo systemctl status nessusd`
 <img width="814" height="493" alt="Check Nessus status and Start up" src="https://github.com/user-attachments/assets/d484468a-4922-4fbb-88ca-bffee924e02a" />

- Open web browser and navigate to: `https://localhost:8834`
- Complete first-time setup wizard: 
   Create admin account credentials.
   Enter activation code from email.
   Wait for plugin downloads to complete Wait for plugin downloads to complete. (this can take 10–20 minutes depending on network speed).

- Successful login to Nessus web interface
- <img width="1090" height="429" alt="login to Nessus" src="https://github.com/user-attachments/assets/d32329f9-86ef-47cf-97ae-84488ef88416" />


---

### STEP 2
**Part 1: Creating AWS account**
- Navigate to Aws https://signin.aws.amazon.com/signup?request_type=register
  <img width="1363" height="592" alt="server creation" src="https://github.com/user-attachments/assets/19a50e16-023f-4247-81a8-2d847ec0b79f" />

- Fill in your email address and check your inbox for code.<img width="1328" height="549" alt="passord aws" src="https://github.com/user-attachments/assets/08f10630-4f40-498b-9132-53bb04845f5a" />

- Fill in your verification code.<img width="1030" height="515" alt="aws code" src="https://github.com/user-attachments/assets/3d4b0368-cee2-4620-90a6-c2766707642b" />

- Enter your password..
- Choose a plan and fill in your payment card details.
- When successful, you should receive a Welcome email.
- <img width="1106" height="552" alt="completion of creating AWS " src="https://github.com/user-attachments/assets/0fbd514b-1c7d-4ea8-a0d0-ec2e80b87688" />



---

### STEP 3 Create the EC2 target AWS
- Before you begin to create your EC2 Target, you need to know what your VM public IP as this will use as IP in your EC2 Security Group to allow only your Nessus VM to reach the instance.
	To check your public IP in ubuntu, Type this command: curl -s https://checkip.amazonaws.com
<img width="853" height="50" alt="My ubuntu public ip" src="https://github.com/user-attachments/assets/e6401c8b-0f7d-40cd-a75f-bfa822afbd58" />


- Save the result for later.

- In the AWS Console, go to the EC2 service and click the Launch Instance button.<img width="691" height="672" alt="Console and clicking Ec2" src="https://github.com/user-attachments/assets/aeb2dcc2-3f5f-4de9-a00e-cb44423ff625" />
<img width="1362" height="649" alt="Ec2" src="https://github.com/user-attachments/assets/6c6ce000-5b8f-43ef-afbf-51ef5657dfbc" />


- Create a name<img width="1343" height="601" alt="Created name " src="https://github.com/user-attachments/assets/29db4cb7-263a-4f43-b7b3-8ca758449084" />

- Choose AMI: Ubuntu Server 22.04 LTS (official)
- <img width="907" height="556" alt="Chose AMI" src="https://github.com/user-attachments/assets/5fa127f1-c2f6-48d8-aee9-95be2cc0d828" />

- Choose Instance type: t2.micro (free-tier)
- On choosing a Key pair: Create key pair and download the .pem file to VM and keep it safe
- <img width="1058" height="556" alt="Key Pair" src="https://github.com/user-attachments/assets/57aa0b71-b52d-4306-b17f-2eb59f7352fe" />

- On Networking Settings:
  <img width="894" height="511" alt="firewall" src="https://github.com/user-attachments/assets/5616e2d8-499a-45b4-94a2-e642b43811a6" />

	- Choose create security group
	- Allow SSH Traffic
	- Replace the source IP with the public IP of your Nessus VM (for example, YOUR_VM_PUBLIC_IP/32)
	- Allow HTTP Traffic from the Internet
	
- Launch the instance and wait for state running.
- In the Instance summary note the IpV4 Public address as that your target ip (EC2)
  <img width="1362" height="565" alt="target ip" src="https://github.com/user-attachments/assets/dbbc9123-5794-464a-8b18-39ca50d3268e" />


---

## Run a Nessus Scan
- Open Nessus in your VM: `https://127.0.0.1:8834/#/`
- Log in to Nessus. <img width="1090" height="429" alt="login to Nessus" src="https://github.com/user-attachments/assets/c7ce87e3-b9cd-4fc0-a1b9-7547f1d37658" />

- From the main navigation, access the Scans section, create a new scan, and choose the Basic Network Scan template
  <img width="1097" height="473" alt="My scans and create new scan" src="https://github.com/user-attachments/assets/394606f8-ec9a-40f9-964c-c1c9c552a6f3" />
  <img width="889" height="381" alt="Basic network scan" src="https://github.com/user-attachments/assets/7e7abaec-7ef9-4779-9cb3-e8b4bcfefbdc" />


- Name: `AWS Target`, `Targets: 16.171.16.0`
 <img width="1089" height="477" alt="Basic scan config" src="https://github.com/user-attachments/assets/842dad6e-96b8-4f32-b4bf-fb84a11272a2" />

- Click Launch, then monitor progress in the ‘Scans’ tab.
 <img width="1300" height="679" alt="Scan Running" src="https://github.com/user-attachments/assets/e0a39616-f745-4e3c-83d0-f537a7caac60" />

- Once complete, export the report as HTML or PDF for documentation. 
<img width="1298" height="489" alt="Scan Completed" src="https://github.com/user-attachments/assets/4c770e34-7099-41be-a66b-bd8ffe2a2cd1" />

## Conclusion
The AWS EC2 instance demonstrated good basic security hygiene, exposing only essential services. 
<img width="1291" height="633" alt="Scan result 303" src="https://github.com/user-attachments/assets/99f703b0-2715-4c1a-bf70-42841c56c2b5" />
<img width="1294" height="696" alt="Scan Results" src="https://github.com/user-attachments/assets/e7578439-5804-44f1-afae-fbe300969ede" />
<img width="1295" height="690" alt="Scan result 101 - Copy" src="https://github.com/user-attachments/assets/fed68b52-4306-4de0-8a9b-7005ddd05421" />
<img width="1302" height="654" alt="Scan result 202" src="https://github.com/user-attachments/assets/32588be8-17b5-4056-9e8d-9a63997c1b55" />
<img width="1302" height="654" alt="Scan result 202 - Copy" src="https://github.com/user-attachments/assets/b07f88e3-26df-46e1-81fd-f005476c4f25" />


**Overall Security Posture:** Satisfactory and no immediate critical threats identified.

## Lessons Learned
- Ensured safe, permission-based scanning practices.
- Understood how network configuration affects scan visibility.
- Learned to interpret Nessus scan results and prioritize remediation.

## References
- Tenable Nessus Essentials: https://www.tenable.com/products/nessus/nessus-essentials  
- AWS EC2 Documentation: https://docs.aws.amazon.com/ec2  

- Nessus User Guide: https://docs.tenable.com/Nessus



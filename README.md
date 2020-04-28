# Cybersecurity Toolkit (CSTK)

### A lightweight toolkit for small companies that cannot afford a large security team.

## Initial GitHub Configuration

> Note: The following steps assume you already have an existing GitHub account.

**Step 1: Create a new repo without a README.**

![Initial GitHub config screenshot](https://lh3.googleusercontent.com/7YV5JHJHscCNhiJIvSFVuxbNcPxxPoB-CKDUp5qNyg5UuIrHLEsUWx3HjD50__6hKHaUCBydezjlJW967bn0CJ0zBCYQhrS6TIWYlPh-pgkf4DUYXYM=w1280)

**Step 2:  Using a Linux distro of your choice, open *Terminal*.**

**Step 3: Create a directory to store submodules (clones of each tool's repo) by entering mkdir CSTK.**

**Step 4: Install Git.**
-   sudo apt update
-   sudo apt install git

**Step 5: Initialize the local directory as a Git repository.**
- git init

**Step 6: Add each project to the local repo using submodules.**
- git submodule add https://github.com/gophish/gophish.git
- git submodule add https://github.com/CISOfy/lynis.git
 
**Step 7: Stage all files for commit.**
- git add .
 
**Step 8: Commit to GitHub.**
- git commit -m "Initial commit"
 
**Step 9: Push to GitHub.**
- git remote add origin https://github.com/yourusername/CSTK.git
- git push -u origin master

# Toolkit download

> Note: Ubuntu Linux was used for this section.

**Step 1: Open *Terminal* and install Git.**
- sudo apt update
- sudo apt install git

**Step 2: Create a folder to download the toolkit to, and then change directories.**
- mkdir CSTK
- cd CSTK

**Step 3: Clone repo locally.**
- git clone https://github.com/yourusername/CSTK.git --recursive

# Using Lynis

> [Lynis](https://cisofy.com/lynis/) is a tool that can be used to perform security audits again a local or remote system. This toolkit will be used to scan remote systems for vulnerabilities. The results from the scan can be analyzed, and remediation can be performed by the company's security team. A follow-up scan should be scheduled to confirm the vulnerability has been fixed.

## Prepare remote system

> Note: Ubuntu server was the OS of the remote system.

**Step 1: From the remote system, create a new user named *securityadmin*.**
 - sudo adduser securityadmin
 - Enter and confirm the new account password.
 - Edit the user information to your preferences.

**Step 2: Make *securityadmin* a sudo (admin) user.**
- sudo usermod -aG sudo securityadmin

**Step 3: Verify the newly created account has the appropriate permissions.**

*The output should be similar to:* 
- uid=1001(securityadmin) gid=1001(securityadmin) groups=1001(securityadmin),27(sudo)

## Scan remote system

> Note: The IP address of 192.168.10.200 below was the address of my test server. Your server IP address will most likely be different, so please substitute in the address of your remote server.
> 
> Steps 2-6 will require you to enter the password for securityadmin.

**Step 1: From *Terminal*, Change the working directory to the subfolder *Lynis* within *CSTK*.**
- cd ~/CSTK/lynis

**Step 2: Create tarball**

- mkdir -p ./files && cd .. && tar czf ./lynis/files/lynis-remote.tar.gz --exclude=files/lynis-remote.tar.gz ./lynis && cd lynis

**Step 3: Copy tarball to target 192.168.10.200**

- scp -q ./files/lynis-remote.tar.gz 192.168.10.200:~/tmp-lynis-remote.tgz

> Note: You will be prompted to enter the account password for securityadmin.

**Step 4: Execute audit command**

- ssh 192.168.10.200 "mkdir -p ~/tmp-lynis && cd ~/tmp-lynis && tar xzf ../tmp-lynis-remote.tgz && rm ../tmp-lynis-remote.tgz && cd lynis && ./lynis audit system"

> Note: You will be prompted to enter the account password for securityadmin.

**Step 5: Clean up directory**

- ssh 192.168.10.200 "rm -rf ~/tmp-lynis"

**Step 6: Retrieve log and report**

- scp -q 192.168.10.200:/tmp/lynis.log ./files/192.168.10.200-lynis.log
- scp -q 192.168.10.200:/tmp/lynis-report.dat ./files/192.168.10.200-lynis-report.dat

## Review scan results

**Step 1: Scroll up in Terminal or open the downloaded log file.**

*If you choose to open the log with a text editor, do the following:*
- cd files
- gedit 192.168.10.200-lynis.log

**Step 2: Take one suggestion from the results and open the URL in Firefox.**
![Terminal output](https://lh6.googleusercontent.com/WR_EoBFoCwfuhQOLzWLyLPAWPwhyGklc0zndaSZPpjjxVMLLe2C2AqleHTb8n0CMbPCKCKPFghncOxzrg3xLMC2Pufkg_U8EqO5WFXSwwXLgXQLk0ZY=w1280)

Follow the recommendations from the link to remediate the issue.
![Suggestion remediation](https://lh6.googleusercontent.com/0pzC8o9zQpgetiVS4rFRUf6ltXWI0X77yRNJeph_HaC04_8hClPtAloYQHP8ywxq4QAz0hzQYbIo1R2H7ZoRPKjgpM2txOkkGOkCN__N5m5LILT6TJQ=w1280)

> Note: This process should be repeated for each suggestion. Not all suggestions are applicable to each company. It is the responsibility of the company's security team to decide if a suggestion is not relevant or if a particular suggestion will not be remediated (acceptable risk).

## Rescan remote system

**Step 1: Repeat all steps from the scan remote system section above.**

**Step 2: Review the results as outlined above.**
- If you not longer have suggestions, your remediation was a success!

# Using GoPhish

> [GoPhish](https://getgophish.com/) is a simple tool that can be used to benchmark a company's exposure to phishing. Users that have clicked on phishing links can have additional cybersecurity awareness training targeted to them. Random, ongoing testing is the best way to help improve the company's exposure to phishing.
> 
> Note: Ubuntu Linux was used for this section.

## Initial setup

**Step 1: From Ubuntu using *Terminal*, run the following commands:**
- cd ~/CSTK/gophish
- sudo go build
- Enter the admin password.

**Step 2: Start GoPhish.**
- sudo ./gophish
- Enter the admin password.

## Initial login

**Step 1: Launch *Firefox*, and navigate to https://127.0.0.1:3333. Ignore any certificate warnings.**

**Step 2: At the login page, the default credentials are as follows:**
- Username: admin
- Password: gophish
![GoPhishLoginPage](https://lh3.googleusercontent.com/KeiDiA7W3ZUfDJ0m8tGouPF-39ZlqtFkKU00aPiB4oJx_tVoXJbP9Mm-_iNfhNIP2jDQC_zRJomPobNfzsGMBST6m_5HAQjhJhOl6yrh1pP-jEBak5I=w1280)

> Note: As a best practice, please change the default admin password. This can be done by clicking the Account Settings link on the left, and then entering your old and new passwords. 

## Create user groups

> By organizing your users into groups, individual departments can be targeted for campaigns. Your message may change based on the group that is targeted. For example, the finance department may receive an email with verbiage about direct deposit changes, while the HR department would receive one about employee benefits.

**Step 1: To create a group, navigate to *Users & Groups* on the left, and then click the *New Group* button.**

**Step 2: A dialog box will appear, which will ask you to name the group and import or add users. Fill out the employee information and click *Save*.**

> Note: Larger businesses with many users may consider exporting all employees that work in a specific department to a CSV file. The template for this action is provided just to the right of the Bulk Import Users button.

**Step 3 (optional): If you forgot or need to add a user, this list can be edited at any time by clicking the pencil icon next to the group.**

![GoPhishNewGroup](https://lh4.googleusercontent.com/aZfz-GPEfY5THKwCebq59YTCbooMJ5UUiAnbXGb-MvKruOKR7YkoSkcthaPfeaA5c3qbQ8YncUpUYEW30_VNFJyZOLewzWII3Ks5OgOjG5cHlCoJnzfI=w1280)

## Create email templates

> Email templates are used to define the contents of the email a user will receive. You can choose to import an existing email or create a custom one from scratch. If necessary, files can be attached to the email.

**Step 1: To create a template, click *Email Templates* on the left, and then click the *New Template* button.**

**Step 2: Name the template and add plaintext or HTML code. Clicking *Add Files* gives you the option to attach files to the phishing email. Optionally, raw email content can be imported by clicking the *Import Email* button.**	

**Step 3: When finished, click *Save Template*.**

![GoPhishEmailTemplate](https://lh4.googleusercontent.com/4va-pWpFi6JoxxUnJU0rlQW94v5cABWjlPc0c4Z5cO0WGvbIEsabxLjs5rv-x7xtDVMzB8-3-soGqV51i4TexsEqt66pK9DRezN1UxmqL_b6xkUprw=w1280)

## Create landing pages

> A landing page replaces the links in a phishing email with your custom page. These pages can be used to capture credentials, redirect users to another website after they submit their credentials, or be created from templates.

**Step 1: To create a landing page, click *Landing Pages* from the side menu. A familiar interface will pop up that is identical to the HTML editing one of *Email Templates*.** 

**Step 2: Input your Custom HTML, or import HTML from an existing URL. Check the boxes next to *Capture Submitted Data* and *Capture Passwords*. You should also define a redirect to page, which matches the user's intended destination. This will help legitimize the email and avoid suspicion from red team assessments.** 

**Step 3: After you have finished, click *Save Page*.** 

![GoPhishLandingPage](https://lh6.googleusercontent.com/LnremkUBZS8pSlbSJLYvfywbdq3dQ57TqHWk74MdzNbLvw3rOnFo9YsrhmZqt0cTKRlcBZcKkA7xgYURNvryA52xhr6KN0aTC74hFpDWeE026OJzemT4=w1280)

## Create sending profiles

> Gophish can handle the email templates and analytics, but is not responsible for email delivery. This makes it necessary to configure an SMTP relay server within the application. Configuring an SMTP relay server is outside the scope of this project.

**Step 1: Navigate to *Sending Profiles* in the left menu and click the *New Profile* button. Fill in all the necessary fields that are appropriate for your environment. You may need to consult with different members of the IT team to gather this information.**

**Step 2: After you have finished, click *Save Profile*.** 

![enter image description here](https://lh4.googleusercontent.com/iWbZQss8XhAJ0O88nTZY7uCMHFM-9463CxvP69PYPFYKLWRRHHhf8eWmgMkMMbdfHd5rl4_mIImkleOZrDePR8L3cLJNJMKWFXs6bt-ruYBR0VOpiEo=w1280)

## Create campaigns and review results

> Campaigns are the core of Gophish. Each profile and template that you have created up to this point are defined here, and they launch at your specified date and time. Multiple campaigns can be setup that are targeted at different user groups within the organization.

**Step 1: To configure a campaign, use the *Campaigns* link on the left menu. Select your previously created templates, profiles, and groups.** 

**Step 2: When you are finished, click *Launch Campaign*.**

![GoPhishNewCampaign](https://lh4.googleusercontent.com/1B5yRXbbdVU1OKNxXNh9-F-ML8Nlls4Ppk-7DFs8WQhthDKzlGD8wdgARROTWFivEjftvRCLwney5GpeVkoasVe3aD4iSzYXISJaDpPnBkwN6aobDCs=w1280)

**Step 3:  A results page will then display that gives you an overview of the campaign. As emails are successfully delivered, the statistics on this page will change accordingly. If you navigate away from this page, it can easily be access again through the Campaigns link on the left, and then by clicking the bar graph beside the desired campaign. If you are satisfied with the results, clicking the complete button on this page will stop the campaign.**

![GoPhishCampaignResults](https://lh3.googleusercontent.com/D9s1Na7XtTh5ljKE-DcmMme00srNxk5G6ALTvxjsxk4DwojXCf2C36Jcm0AjjrGqIFmNh-RsSKnecu97S06SQKpv42MCxstfGL9cOr4Sl6JFlye9xu0=w1280)

# Using OPNsense

> [OPNsense](https://opnsense.org/) is a open source software that provides firewall and routing capabilities. Many of its features match that of expensive commercial firewalls, and in some cases surpass them. Prior to starting this section, you should follow the installation guide located [here](https://opnsense.org/users/get-started/). For small to medium companies, it is recommended to run this software on physical hardware. The recommended specs can be found in the previous link.

## Importing the default configuration and backups

**Step 1: From your Ubuntu device, navigate to the LAN interface IP address of https://192.168.1.1**

**Step 2: Log in using the following credentials:**
- Username: root
- Password: opnsense

**Step 3: From the left menu, select *System* > *Configuration* > *Backups*.**

**Step 4: In the *Restore* section, click the *Browse* button under *Restore area*.**

**Step 5: In the *File Upload* dialog box, browse to *Home* > *CSTK* > *SampleConfigs* > *firewall*. Select the file *config-OPNsense.template.xml*, then click *Open*.**

**Step 6: Click the Restore configuration button in the Restore section. Your firewall will now reboot with the template configuration.**

**Step 7: To backup, select Download configuration. The browser will prompt you to save it to a location of your choice. Optionally, automated backups can be setup to Google Drive or Nextcloud.**

> Note: This configuration should be used as a guide only. Each customer's network will have its own unique setup.

![OPNsenseImport](https://lh6.googleusercontent.com/vVpqcS1Dy0qnhVvEfMKobVHY3zlqe5-qJ7ZHHUTx6YOjAH036Y0CVYt3WtqAgCVErucB8BVYBE-NxkuJRbUegHFbx2b1G0zmX1zeyvA5kIpVbYLZOIU=w1280)

## VLAN configuration

> As a best practice, networks should be segmented for the purposes of security and breaking up broadcast domains. Many small to medium companies may not have this level of segmentation. The VLANs that were imported from the configuration should be used as a guide only. Each network will be different.

**Step 1: To view, edit, or add a VLAN, use the left menu and navigate to *Interfaces* > *VLAN*. **

**Step 2: To edit an existing VLAN, select the *pencil icon* next to the VLAN. You can then change the parent interface, tag, priority, and description to fit your needs. After you are finished editing, click *Save*.**

**Step 3: To add a VLAN, select the *Add* button to the right. Enter the desired parent interface, tag, priority, and description to fit your needs. After you are finished editing, click *Save*.**

![OPNsenseVLANConfig](https://lh6.googleusercontent.com/sb77wtec4FqFaVT99CeDnRlKSgGNPrOG5xg4hlV2B14dWyQBgiH2B7jOuFV_Kby65aeq8z7S6kAvbuGLVDU7iFtluUopk4-Z-GHSxqbGIvHNGkqa3skF=w1280)

## Creating firewall rules

> Firewall rules help to prevent clients from access resources they should not be using. Additionally, they help to prevent DDoS attacks, botnets, and bad actors from affecting your network. This section will focus on how to block DNS queries unless they are to a trusted server.

**Step 1: To view, add, or edit a firewall rule, use the left menu and navigate to *Firewall* > *Rules* > *LAN*.**

**Step 2: To add rule, select the *Add* button in the top right of the window. You can then set the default *action*, *direction*, *source*, *destination*, along with many other settings. To allow DNS queries only to trusted sources, configure your settings to match the screenshot below. Note: you will need to change the destination address of the DNS server to match your own.**

![OPNsenseFirewallDNSRule](https://lh5.googleusercontent.com/no_QBfW43mQhphLWlUaVZOOfCbPpIw3pd-PiMUK8LC3vBVxWUKv3TdkX4DBXpFej5onsQ38wtKWhLBTST50Inol5QRK-2LNpQU63Z_x4vy4MmJtrFHE=w1280)

**Step 3: To edit an existing rule, click the *pencil icon* beside the existing rule. The interface will look the same as step 2. Make your desired changes, and then click *Save* when you are finished.**

**Step 4: Click the Apply changes button to save your firewall rules to memory.**

![OPNsenseApplyChanges](https://lh5.googleusercontent.com/0J6p7l0aFhIK9oJHS9t9WGdcm8cOoBOJaAq4KyAT0uV_zupD_qPcuwATy10QytEgbmURVmPXyuKSRSmq7NpyVAktmMtbKeDfNtmhXqA0ULgO5l2vkbMD=w1280)

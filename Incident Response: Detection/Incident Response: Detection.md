# Incident Response: Detection
- This file will contain a lab for incident response
# Scenario
- In this lab, you will learn about using an automated security platform, wazuh, to detect IoCs related to suspicious activity. This lab demonstrates the detection phase of an Incident Response Plan.
- As a security professional, you want to take full advantage of automation to detect and potentially respond to security violations. In this lab, you will use wazuh to review security alerts (i.e., detections) related to questionable logon activity. Finally, you will delete audit logs files and then use wazuh to evaluate the detection of this abusive activity.
- Your security workstation, running Kali Linux, is located in Structureality's server subnet. You will access the wazuh web interface from Kali and DC10 while performing attack simulations from Kali and DC10 against DC10.
# Understand your environment
- You will be working from a virtual machine named KALI hosting Kali Linux. This system is your security workstation and is located in Structureality's server subnet. You will use a virtual machine named WAZUH running Ubuntu Server and supporting the wazuh security platform. You will be accessing the wazuh web interface from Kali. You will also be using a virtual machine named DC10 hosting Windows Server 2019, where you will perform attack simulations on and against.

- In a kali linux machine perform a password spraying attack
- Open a terminal and type in sed '57i\Pa$$w0rd' /usr/share/seclists/Passwords/500-worst-passwords.txt > passlist.txt
- Here is what the command does:
- sed — a text-processing utility that can modify or filter text.
- '57i\Pa$$w0rd' — the sed instruction:
- 57 means line 57.
- i means insert.
- Pa$$w0rd is the text being inserted.
- /usr/share/seclists/Passwords/500-worst-passwords.txt — the input file, a seclists password wordlist.
- > — redirects the command's output into a file rather than displaying it on the terminal.
- passlist.txt — the resulting output file.
- In other words it takes 500-worst-passwords.txt, inserts Pa$$w0rd immediately before the existing line 57, and saves the modified list as passlist.txt, that password is going to be used later in the lab
- To confirm that the password was added correctly, type in the following command: grep -n 'Pa$$w0rd' passlist.txt
- The results should be 57:Pa$$w0rd meaning that the password was added to the password list file

# wazuh
- To view the security events of only DC10 system, click on Security events from the Security Information Management section of the wazuh home page
- Select Explore agent and select DC10
- Scroll down the Security events page to view the currently available information
- Scroll back to the top of the Security event page and select Refresh to update with new events

# Performing a password attack
- Open a terminal in Kali Linux and type in the following command: hydra -t 1 -V -f -l administrator -P passlist.txt rdp://10.1.16.1
- This command will perform the password attack against the target, it will succussed on the 57th attempt and hydra will terminate once a successful password guess occurs

# wazuh
- Switch back to wazuh
- Select Refresh again at the top of the page
- View the security alerts resulting from the password guessing attack
- Find the entry of Rule ID 92652 for the successful password discovery
- You should find incremented Authentication failure and Authentication success
- Type in 92652 into the search field at the top of the page and select Update
- Click on refresh or update button again
- Scroll down to view the list of Security Alerts
- You should see only 1
- Select on the event for Rule ID of 92652
- Rule ID 92652 means a user successfully logged in remotely via NTLM
- The wazuh security events page will present a range of interesting information for each listed event, including Techniques, Tactics, Description and Leve
- View the Technique information related to Rule ID 92652
- Select T1550.002 from the first Security Alerts row of an entry with Rule ID 92652
- You should see a Detailed page about the Pass the Hash technique is diplayed
- Go back to Wazuh and select Security Events in the Security information management section, type in 60122 and select update
- Rule ID 60122 means logon failure, there should be many of these failed attempts

# Kali Linux
- Open a new terminal
- Enter the following command to create a mount point: mkdir /mnt/dc10-c
- Enter the following provide Pa$$w0rd as the password when prompted.
- Type in: mount -o username=jaime //10.1.16.1/c$ /mnt/dc10-c
- This mount should fail
- This time attempt to mount the C$ share using administrator account and Pa$$w0rd as the password
- Type in the following command: mount -o username=administrator //10.1.16.1/c$ /mnt/dc10-c
- This mount will succeed

# wazuh
- Locate the security events caused by the mount attempts
- The Rule ID for a logon failure is 60112
- The Rule ID for a logon success is 60106
- Select Refresh to update wazuh Security events
- Enter 60112 in the search field and then select Update
- Select an alert to review the details
- Enter 60106 in the search field and then select Update
- Select an alert to review the details

# Conclusion
- You have triggered and seen wazuh security alerts trigged by matching IoC's to questionable logon activity
- The activity of incident response detection is the recording of events into logs, the automated analysis of those logs, and the automated notification of significant incidents

# Detecting ant-forensics with wazuh
- Anti-forensics are activities performed by intruders in an attempt to mask, hide or destroy evidence of their malicious action on a sytem
- We will perform anti-forensics by deleting log files and view the related security alerts of these IoC's in wazuh

# Windows/Event Viewer
- Type in Event Viewer in the search bar at the bottom right
- In the left panel double click Windows logs to expand it
- Click on Security and on the right panel select Clear log....
- On the Event viewer pop-up window click on Clear

# Wazuh
- Locate the wazuh security alert related to the security log deletion Rule ID 63103
- Enter 63103 in the search field and then select Update
- The description should be The audit log was cleared for Rule ID 63103
- This indicates the detection of IoC's of clearing logs of a monitored system through wazuh








































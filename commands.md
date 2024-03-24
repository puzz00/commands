# Commands

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

## tape archive

*tar* is used to archive files and directories. It does not have to be compressed but it can be.

Archive files are simply a collection of files which have been archived into one file - they can be unarchived back into their original form.

`sudo tar -cvf file.tar *` | the -c flag creates an archive | the -v flag is for verbose output | the -f flag lets us specify the archive filename | the * wildcard specifies all the files in a directory

`sudo tar -xvf file.tar` | the -x flag specifies extract all the files to their original form

`sudo tar -zcvf file.tar` | the -z flag specifies compress the archive file using *gzip*

`sudo tar -zxvf file.tar` | this command decompresses and extracts the compressed and archived files

## arp spoof

Before we run a *man in the middle* attack, we need to configure our machine so traffic can be forwarded.

`echo 1 > /proc/sys/net/ipv4/ip_forward` | configures the machine to forward traffic

We can now spoof the machines we are targetting. One of these machines will be the gateway.

`sudo arpspoof -i eth0 -t 172.16.5.30 172.16.5.1`

`sudo arpspoof -i eth0 -t 172.16.5.1 172.16.5.30`


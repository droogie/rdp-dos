# RDP DoS

This is a denial of service against RDP tested on the latest Windows 10 Version 1803 (OS Build 17134.165)... this was also verified against Windows Insiders build for Win10 and Windows Server 2019.

Machines that do not enforce NLA can be exploited without unknown credentials, where NLA enforced machines would require valid credentials to DoS.

I did not spend much time investigating this, but I can make a guess at what is happening. I believe that the service receives a message from our client that it does not like and terminates our connection, but does not clean up and free the memory that was allocated. If you run this client in a loop you will see the memory usage climb exponentially until ultimately putting the machine into an out of memory state. This tends to BSOD instances running on VMware due to their graphics driver freaking out due to no memory available... 


If you're interested in the protocol: https://winprotocoldoc.blob.core.windows.net/productionwindowsarchives/MS-RDPBCGR/[MS-RDPBCGR].pdf


# Build

This was verified on Ubuntu 18.04 and Kali 64bit... the shell script will install dependencies, patch the necessary files then build the modified freerdp client.

Requires root to install dependencies... if you already have freerdp installed this will overwrite it.

sh build.sh  

# DoS

tls: supply anything for credentials, does not matter...
nla: requires valid credentials

sh dos.sh <ip> <username> <password> <tls/nla>

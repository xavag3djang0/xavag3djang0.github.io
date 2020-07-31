---
title: Bounty Hacker Writeup
published: true
---

# ***TryHackMe*** - Bounty Hacker : You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker!

Link to THM Room : [Bounty Hacker](https://tryhackme.com/room/cowboyhacker)

## About the room : 

Pretty basic room, as the title suggests Bounty Hacker, the theme is quite fun. Just think you're the guy sitting at a bar and boasting about your h4cking skills, and then you're recruited for something Epic. Thus, time to show your skills for real rather than just talks or you loose all the spotlight you created.... LOL! <br>
So, time to blend in and show-off some skills.... **Let's Hack**

### Methodology :

- **Port scan to find open ports and enumerate**
- **Brute-force with Hydra**
- **Exploitation and Privilege Escalation**

#### Port scan - nmap

>> nmap -sC -sV -Pn -oN bounty 10.10.x.x

<img src="/images/nmap_bounty.jpg">

Ok, viewing the nmap results it shows mainly 3 ports open.<br>
Port 21, 22, 80<br>
Hope we all know what these ports are about.<br>

Quickly hopping into the browser to check the web-server running on port 80.

<img src="/images/site.jpg">
***Viewing the source code of the site*** 

Looks like a normal html site, nothing too interesting here, only the story-lines getiing catchy about our skills.<br>

We had **Port 21** open and can see Anonymous FTP login is allowed, so let's enumerate that...<br>
Back to terminal and we login to FTP.<br>

>> ftp 10.10.x.x

Enumerating here, we find two files, locks.txt & task.txt<br>
Let's get them and check what we got...

<img src="/images/ftp_bounty.jpg">

Now, viewing the downloaded file in editor, **locks.txt** consists of list of strings or words, possibly a password dictionary which we can use to brute-force user's password.<br>

<img src="/images/locks_lin.jpg">

Another one **task.txt** contains a list of task left by a user called **lin**

<img src="/images/task_lin.jpg">

Since, we got the file from the system we were assigned to break into and the name "lin" fits to our answer in THM task, this is our target user to try brute-force on. <br>

#### Brute-force with Hydra

Well let's use the wordlist and the user we found to fit into SSH credentials.

>> hydra -l lin -P /your/path/to/**locks.txt** ssh://10.10.x.x

<img src="/images/hydra_haha.jpg">

Cool! Hydra got the job done.

#### Exploitation and Privilege Escalation :

Since we got the credentials required, time to exploit and get the flags...

>> ssh lin@10.10.x.x

<img src="/images/sshhh_success.jpg">

Enumerating here we find the **user.txt** lying on bare sight on /Desktop folder.<br>

Checking for the sudo privileges for the current user.

<img src="/images/sudol.jpg">

Sweet, lin can run /bin/tar as root.<br>
Tar is a linux command which can be used to create, compress and uncompress Archive files and also modify them as well.<br>

GTFOBins got the way we need to exploit this privilege(feel free to use any of your skill to manipulate tar and get it done) and hence, escilate our privilage to **root**, google it or just click here [GTFOBins](https://gtfobins.github.io/gtfobins/tar/)<br>

Run the following command, should get you a privileged(root) shell, or feel free to use any other techniques mentioned.

>> sudo /bin/tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

Bingo ! we in as root user, let's just cat the "root.txt".

<img src="/images/root_access.jpg">

And thus, success !!! Rooted the machine and got to keep our title as Elite Bounty Hacker, our fellow team on the deck must have admitted and appreciated our H4cking skills... ;-)

Indeed easy and fun machine to play with and practice our skills.<br>
*Thank you for reading, keep up the hack !!!* 
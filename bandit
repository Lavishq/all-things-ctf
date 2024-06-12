## Level 0
`ssh bandit.labs.overthewire.org -p 2220 -l bandit0`
promts for password `bandit0`

## Level 1
the cmds mentioned are `ls , cd , cat , file , du , find`
i tried `ls` then `cat readme` since only one file was present and it gave me a password `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`
i also tried `du` which output 20 and in my local machine it took a minute to run and has more than `100k`
> i might need to investigate `du` in future but i use `df -h` which is is a wrapper around du for storage and memory

## Level 2
logging to level 2 using password of level 1 

`ssh bandit.labs.overthewire.org -p 2220 -l bandit1`
promts for password `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

`cat  ./-` to open the dashed file and prints this output `263JGJPfgU6LtdEvgfWU1XP5yac29mFx` 
 
 > can also use `cat < -`

## Level 3
login as bandit2 using pass^
`cat "spaces in this filename"`
> can also do `cat s`  press tab and autofill `cat spaces\ in\ this\ filename`
output: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

## Level 4
login as bandit3 using pass^
`cat inhere/` press tab and autofills `cat inhere/...Hiding-From-You` 
output: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

## Level 5
do as b4 but inhere has many files so cat then one by one and then ... do in decending order until readable pass in found
`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

## Level 6
the conditions are 
```
human-readable
1033 bytes in size
not executable
```
idk how to find if it is human readable but file size maybe `ls -al` can do and not executable will be visible by that

i realized it will be inefficient to look for so many dir/ so thought of using some shell script or `du`, `find`... then i googled and found the answer
`find ./ -type f -size 1033c ! -executable`
outputs `./maybehere07/.file2`
`cat ./maybehere07/.file2`
outputs: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

## Level 7
login 
`find / -type f -user bandit7 -group bandit6 -size 33c`
outputs too many permission denied and `/var/lib/dpkg/info/bandit7.password` so `cat` it
output `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`
> `find / -type f -user bandit7 -group bandit6 -size 33c 2> /dev/null` so So, 2> /dev/null means that, redirect the error output from this command to /dev/null. Essentially meaning, just get rid of the error messages

## Level 8
The password for the next level is stored in the file data.txt next to the word millionth

cmds
`man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd`

i knew i had to `grep` but i cat the file and `ctrl c` couldnt stop it, i exited the terminal relogin

`cat data.txt | grep millionth`
output: `millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

## Level 9
login, password occurs only once in data.txt

`grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd` cmds to solve and tar and zip would not be used

it also had [Piping and Redirection](https://ryanstutorials.net/linuxtutorial/piping.php) and i skimmed it and ran `wc -l data.txt` and remembered it is just word count...

i cheated and `sort data.txt | uniq -c | grep '^ *1 '`
ouput: `1 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`
> alternative elgant, i only looked at 2 solns: `sort data.txt | uniq -u` outputs `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

## level 10
again i cheated but first i had the thought of `cat <f> | grep "*==*"`, since it was mentioned that several `=` were used and i thought using `==` will get me quick ans but...
so i cheated and used `strings data.txt | grep '^='` for efficiency

output:
```
========== the
========== password
=b.(
========== isv
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

## Level 11
this was ez, i didnt cheat or even open base64 wiki
just do `base64 -h` and errored `base64 --help` worked saw options -d for --decode and -i for --ignore-garbare
cmd
`base64 -d -i data.txt`
o/p: `The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

## Level 12
i had a `fuck` moment when i read  
```
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
```
i knew i had to read it while rotated but which direction and what tool to use for it, the best way would be to cheat

so i cat the normal text and i got `Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4`, first thought is this german?

and then i checked if the `whatis tr` to see this will be used and indeed `tr (1)               - translate or delete characters`
checks helps doesnt understand and cant spend more time so goes for solution since `efficiency` 

`alias rot13='tr a-zA-Z n-za-mN-ZA-M'`
`cat data.txt | rot13`
hm, i got the answer but i still dk how this worked,
i mean `tr a-zA-Z n-za-mN-ZA-M` looks like `a-z` will be `a-m` and `A-Z` ... idk idc, lets move on and learn it when the use comes up

`The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

## Level 13
it has too much information and idk why it wants to create a new dir (hard to guess) in `/tmp`, cant `ls` in `/tmp` was able to `cd /tmp` then `mktemp -d` as it stated and `cd` into it then copied `cp /home/bandit12/data.txt .` but didnt rename as stated... now there is `hexdump` wikip- link... and is being repeatedly compressed as stated in level goal

i thought `tar` will be used but i never checked `xxd` so did `xxd` and this is it 
`xxd (1)              - make a hex dump or do the reverse.`
i ran it on the file `xxd data.txt` and got well `hex`
then `xxd --help` and `-r for reverse` 

did `xxd -r data.txt` and got unreadable text so i knew decompression needed, and from cmds `gzip, bzip2, tar` would be used so i will go over `whatis` for all of them... looks like `gzip` for compress/expand

next `xxd -r data.txt > eata.txt` then `gzip eata.txt` then `ls` and i see `data.txt  eata.txt.gz` ... i was supposed to decompress so check --help, gets `-d` then `gzip -d eata.txt.gz` and ls gives `data.txt  eata.txt` then `gzip -d eata.txt` gets `gzip: eata.txt: unknown suffix -- ignored`
probably bc file didnt recognize that it need to be `.gz` extension so i use `mv eata.txt eata.txt.gz` and decompress again but still the ... file is unreadable and go for solution since idk... i can try to hack away but i have to do many more things in a day so i cant be stuck now

> finds a solution
`rm eata.txt`
> here is the steps
`cat data.txt | xxd -r > data` 
`file data`
`mv data data2.gz`
`gzip -d data2.gz`
`ls`
output: data2  data.txt
`file data2`
output: data: bzip2 compressed data, block size = 900k
`mv data2 data3.bz`
`bzip2 -d data3.bz`
`ls`
output: data3  data.txt
`file data3`
output: data3: gzip compressed data, was "data4.bin"....
`mv data3 data4.gz`
`gzip -d data4.gz`
`ls`
output: data4  data.txt
`file data4`
output: data4: POSIX tar archive (GNU)
`mv data4 data5.tar`
`tar -xf data5.tar`
`ls`
outp: data5.bin  data5.tar  data.txt
`file data5.bin`
o: data5.bin: POSIX tar archive (GNU)
`mv data5.bin data6.tar`
`tar -xf data6.tar`
`ls`
o: data5.tar  data6.bin  data6.tar  data.txt
`file data6.bin`
o: data6.bin: bzip2 compressed data
`mv data6.bin data7.bz`
`bzip2 -d data7.bz`
`ls`
o: data5.tar  data6.tar  data7  data.txt
`file data7`
o: data7: POSIX tar archive (GNU)
`mv data7 data8.tar`
`tar -xf data8.tar`
`ls`
o:data5.tar  data6.tar  data8.bin  data8.tar  data.txt
`file data8.bin`
o:data8.bin: gzip compressed data, was "data9.bin"
`mv data8.bin data9.gz`
`gzip -d data9.gz`
`ls`
o:data5.tar  data6.tar  data8.tar  data9  data.txt
`file data9`
o: data9: ASCII text
`cat data9`
finally, `The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

writing this was more diff than running cmds...due to accuracy and i was going in right direction when i started solving just that missed use of `file <>` and repeating these until `p`

## Level 14
i tried to ssh via bandit13 shell but got the `text` displayed below

copied the pk and saved locally and tried to login and was getting the same text displayed below
```
bandit13@bandit:~$ ssh bandit14@localhost -i sshkey.private
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
then looked for soln, and [he did the same as me and succeeded](https://medium.com/secttp/overthewire-bandit-level-13-dd14fd9aa3b2) 
found [another solution](https://mayadevbe.me/posts/overthewire/bandit/level14/)
and solved it, well the only thing i had to do was `chmod 700 sshkey.private` then `cat /etc/bandit_pass/bandit14` 
output: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`
though, i learnt a couple things from this challenge

> the breakdown of learnings, 
```Now that I had the private ssh key, I tried to log in with it. However, I got the following warning Permissions 0640 for 'sshkey.private' are too open., because it had the following writing permissions: -rw-r-----. So I reduced the permissions with chmod 700 sshkey.private, so only the owner (me) has permissions for the file. The permissions will then look like this: -rwx------. And now it is possible to use the key to log into the new level:

```
^pasted directly from his blog

another learning was to be able to donlod the sshkey... first i copy and pasted it after cat, but `scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .`

ps: cmds to use were `ssh, telnet, nc, openssl, s_client, nmap` and im famaliar w/ them but not too much

## Level 15
i think i need to do post the password on port 30000 on their localhost ... i probably believe that among `ssh, telnet, nc, openssl, s_client, nmap` these `nc` should be used as i have seen in other ctfs for posting flags

lets cheat, i dont need to use the password that i received since im already logged in due to previous pk login

using `whatis nc` then `nc --help`
then `nc localhost 30000` and pasting the pass gets

outp: 
```
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

> also, looked at one soln, and
`echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000`

## level 16
login,
we have to do what we did b4 but w/ ssl encryption and -p 30001
`nc --help` and `-M ttl` i guess this should be it

```
bandit15@bandit:~$ nc localhost 30001 -M ssl
nc: ttl is invalid
bandit15@bandit:~$ nc localhost 30001 -M SSL
nc: ttl is invalid
```
did above block, [opened the helpful reading material](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html) and dont want to spend 20 minutes reading this

how dumb i can be for not noticing openssl cmd in the mentioned cmd,
`openssl s_client -connect localhost:30001` enter,
paste `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

or, this looks convoluted,
`echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -connect localhost:30001 -ign_eof` 

output: 
```
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```
ps: bookmarked the openssl book for future in case it will be req or needed to be handy

## Level 17
first thing i see is `port scanner on wikip-` in helpful-
so my answer is `nmap` but need to read the goal now...

after reading the goal i think these solns would be ez w/ for loops in py, duh -
`nmap --help` overwhelming and reading all will...

i think somehow using nmap for the range 31000..=32000 and then also piping them to see if the have server listening and then those who speak ssl.. submit

im cheating... since it would be more fun this way

`nmap -sV localhost -p 31000-32000` i let this run while i write, `-sV` is for service/version detection


bandit16@bandit:~$ `nmap -sV localhost -p 31000-32000`
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-12 10:54 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00015s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```
hm, i tried `openssl s_client -connect localhost:31518` and pass and was getting `KEYUPDATE` or `my input` echoed since this is ssl/echo service

next `openssl s_client -connect localhost:31790` and pass but didnt get the res 

finally used `echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:31790 -ign_eof`
to and got correct w/ `pk`

## Level 18
used vim to paste into previous sshkey.private,
loggedin `ssh bandit.labs.overthewire.org -p 2220 -l bandit17 -i sshkey.private`

2 files password.old and password.new
idk, what this means yet from goal of this level
`NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19`

first thing `cat password` tab and do one.. and i see a long hex i guess
then `diff password.xxx password.yyy` 

i find these diff
```
< 7l8XDIvIk4Xtl7jMkwSKw6wYEedt6lcp
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

## Level 19
opens a new tab and try to login to bandit 18 while staing keeping bandit17 tab on and use the above diff passes, old didnt login, new did but see `byebye as was in note`

now i open the [challenge](https://overthewire.org/wargames/bandit/bandit19.html) and wow, .bashrc was modified to log out... i think i can close bandit17 tab and focus on bandit18, this is cool trick that can be used to annoy your friends and they have to either wipeout their disk or chroot and fix it i guess, lol

cheated and the soln was simple, where you can execute cmd via ssh
`ssh bandit18@bandit.labs.overthewire.org -p 2220 <cmd>`
`ssh bandit18@bandit.labs.overthewire.org -p 2220 ls`
`ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme`

outputs: `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`

> Alternative, using same method as above we can spawn a bash shell or use -t to open a pseudo terminal that runs on target machine, this is useful for runnign multiple commands since repeat in ssh logins
`ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/bash`
`ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh`

i have a doubt tho, since we run `/bin/bash` why doesnt it run .bashrc? is it cause we are directly running binary- maybe investigate it later

## Level 20

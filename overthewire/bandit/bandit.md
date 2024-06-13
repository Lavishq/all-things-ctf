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
---
day2 of doing bandit ctf and i will cheat more this time... and do it fast

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
login, ls -la
`./bandit20-do`
outputs: 
```
Run a command as another user.
  Example: ./bandit20-do id
```

`/bandit20-do id`
outputs: `uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)`

`./bandit20-do cat /etc/bandit_pass/bandit20 `
outputs: `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

> learnt something new, `Suid is a special permission. It will replace the x of the user permission. It means the binary will be run as the owner of the binary, not the one executing it. To give a binary suid permissions the following command needs to be used: chmod u+s <filename>`, idk if i knew about suid or forgot but it feels a new concept and here is the [source](https://mayadevbe.me/posts/overthewire/bandit/level20/) is read from.

## Level 21
new thing i learn here is `-n flag prevents newline` and i already knew about `&` is used for sending proc in bg

`echo -n '0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO' | nc -l -p 1234 &`

`./suconnect 1234`

output:
```
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

btw, `ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)` these were the necessary cmds so maybe it couldve used multiple terminal via tmux or tabs and achieved them without `&` but imma not do that

## Level 22
cmds, `cron, crontab`
crons, `cat /etc/cron.d/` press tab and outputs some stuff then `cat /etc/cron.d/cronjob_bandit22` 
output: 
```
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

cat the above .sh file via `cat /usr/bin/cronjob_bandit22.sh `
```
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
i thought it was password for next level and used it for next level but it is not and i failed to login, it is a file that contains the password so cat it using `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`

output: `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`

> another thing i noticed is you can login using `ssh <user>@bandit.labs.overthewire.org -p 2220`

## Level 23
follow prev. steps and this is a bash script w/ a md5 hash as password in another file

well you can run each line one by one to see what they do, 
```
bandit22@bandit:~$ whoami
bandit22
bandit22@bandit:~$ $(echo I am user $myname | md5sum | cut -d ' ' -f 1)
7db97df393f40ad1691b6e1fb03d53eb: command not found
bandit22@bandit:~$ echo I am user $myname | md5sum | cut -d ' ' -f 1   
7db97df393f40ad1691b6e1fb03d53eb
```
so i thought the password would be in `/tmp/7db97df393f40ad1691b6e1fb03d53eb`

but we have to use `bandit23` since the ctf of bandit22 doesnt have a file or dir/ and bandit23 output does `echo I am user bandit23 | md5sum | cut -d ' ' -f 1`
`cat /tmp/8ca319486bfbbc3663ea0fbe81326349`
output: `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`

## Level 24
cmds `chmod, cron, crontab, crontab`, and also we need to create a .sh script

well, i looked at `cat /etc/cron.d/cronjob_bandit23` and then `cat /usr/bin/cronjob_bandit23.sh` had code same as prev. so i looked for `cat /usr/bin/cronjob_bandit24.sh` and it gave a bash script

```
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

well my understanding of bash is not high but okayish, so in for loop i think it is looping upto the no. of files `/var/spool/$myname` has since echo.. and then it has if check ... and echo and removes the file... as mentioned

now, i tried to run the lines one by one and i got error for no such file or /dir for `/var/spool/bandit23` so i tried `bandit24` and it worked
`ls -a` 
o: `.  ..  foo`
hm, the if check is doing something `if [ "$i" != "." -a "$i" != ".." ];` w/ `current dir/` or `parent dir/` not eq, stores owner and if owner is bandit23 then it runs every 60 seconds and `-s is signal flag and signal 9 is kill iirc` and then removes the file... and `stat --format "%U" ./$i` line idk what itis but i ran it and it displayed `root` and then `whatis stat` outputs

```
stat (1)             - display file or file system status
stat (2)             - get file status
stat (3type)         - file status
```

well, i got tired of investigating it for more decadous min and cheat-
he used `mktemp -d` but i cant tab or typing the random characters is a drag ...  i tried to use `mkdir /tmp/lopl` then  `cd` into it then `vi script.sh`

`chmod 777 script.sh`
pasted this into it
```
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/lopl/pass
```
`touch pass`
`chmod 777 pass`
`chmod 777 .`
`cp script.sh /var/spool/bandit24/foo/`

> this was a little difficult, but i did it 

```
bandit23@bandit:/tmp/lopl$ cat pass
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

## Level 25
a daemon, 
writing a script [from source](https://mayadevbe.me/posts/overthewire/bandit/level25/)

`mkdir /tmp/b24` cd into it
`vi brute_fp.sh`
paste
```
#!/bin/bash

for i in {0000..9999}
do
        echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> possibilities.txt
done

cat possibilities.txt | nc localhost 30002 > result.txt
```
`chmod +x brute_fp.sh`
`./brute_fp.sh`
`sort result.txt | grep -v "Wrong!"` -v is invert flag meaning it excludes "Wrong!"
but i got this output
```
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
```

so i ran this line and got the following output,

```for i in {0000..9999}; do echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i; done | nc localhost 30002```

```
Wrong! Please enter the correct current password and pincode. Try again.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

## Level 26 
i did `ls` and found the pk for bandit26 but lets read more,
after reading q, i looked at soln, and read the line, `The information, what shell is the default for a user, can be found at the end of the line for the user in the ‘/etc/passwd’ file` then ran the cat both locally and on sshed login `/usr/bin/showtext` was then shell for bandit26

cmds for this level are `ssh, cat, more, vi, ls, id, pwd`

i continued reading, `more is a shell command that allows the display of files in an interactive mode. Specifically, this interactive mode only works when the content of the file is too large to fully be displayed in the terminal window. One command that is allowed in the interactive mode is v. This command will open the file in the editor ‘vim’.` didnt understand much but know that visual, insert, normal modes are in vim so i might need to investigate interactive mode of shell or whatever ... idk

ik some of it since i had to use but dont usually remember cmds, so pasting here `It is possible to use vim to break out of a restricted environment and spawn a shell. To spawn the user’s default shell, the command :shell is used. To change the shell to ‘/bin/bash’ the command is :set shell=/bin/sh.`

following his solution,
`cat /usr/bin/showtext`
o:
```
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```
the script reers to a file `text.txt` which is probably in /home/bandit26/

```
bandit25@bandit:~$ whatis more
more (1)             - display the contents of a file in a terminal
```
cat the pk found initially and copy, logout, paste pk in a file
running locally,
```
❯ ls -l sshkey.private
.rwx------ 1.7k lavishq 13 Jun 18:10  sshkey.private
```

`chmod 700` and `ssh bandit.labs.overthewire.org -p 2220 -l bandit26 -i sshkey.private` and i get logged out w/ `Connection to bandit.labs.overthewire.org closed.` msg 

and the solution im referring says, `What exactly has happened? The text in ’text.txt’ is very short, meaning the whole text can immediately be displayed. more does not need to go into command/interactive mode. If we make the terminal window smaller, more will go into command mode. We can then use v to go into vim. Now we can rescale the window.`

In my case i tried to do minimization of only 1 line display but still got logged out... i tried it w/ even less and it worked the pressed `v` and entered vi, i ran `:r /etc/bandit/pass/bandit26` well i used tabs to autocomple-

output: `s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ`

> more on it, `Vim is now opened as bandit26 and we can do different things to retrieve the password. With :e /etc/bandit\_pass/bandit26 we can open the password file and read the password. If we want a shell, we could try the :shell command that vim offers. This command, however, uses the user’s default shell. What we need to do instead is to set the default shell of the user in vim to a useful shell, like \bin\bash. The commands look like the following: :set shell=/bin/bash and then use :shell. Finally, we have a shell and can get the password for the user. cat /etc/bandit\_pass/bandit26` pasted by [source](https://mayadevbe.me/posts/overthewire/bandit/level26/)

## Level 27
using `more on it` ending part to login again, 
then `:set shell=/bin/bash ` then `:shell`
`ls`
o: `bandit27-do  text.txt`

here is following
o: 
```
bandit26@bandit:~$ ./bandit27-do 
Run a command as another user.
  Example: ./bandit27-do id

bandit26@bandit:~$ ./bandit27-do id
uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)

bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

## Level 28
login, this is git
`mkdir 27`
`cd 27`
`git clone ssh://bandit27-git@localhost/home/bandit27-git/repo`
it doesnt clone so i look at the goal again and 
`git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo` then it asks password and i enter the level pass
```
bandit27@bandit:/tmp/27$ cat repo/README 
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

## Level 29
login, did like b4 `mkdir 28` cd then git clone w/ port and cat readme gave
```
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
now what, go look at cheats ... solution had explainer of `git log` and `git show <commit>`
so i got my hints, lets continue `no cheats`

log commits
```
commit af334669319406775f87c8f54cc3e22df09a4f8c (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Jun 11 21:29:56 2024 +0000

    fix info leak

commit dd82249be85fc9f61c7ad9f5bbc49e41df0f0503
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Jun 11 21:29:56 2024 +0000

    add missing data

commit 65a35069b7edd886c0babb1297a177727123da29
Author: Ben Dover <noone@overthewire.org>
Date:   Tue Jun 11 21:29:56 2024 +0000

    initial commit of README.md
```

hm maybe use git show, one by one

`git show <>` ran the first commit hash and got the pass

```
bandit28@bandit:/tmp/28/repo$ git show af334669319406775f87c8f54cc3e22df09a4f8c 
WARNING: terminal is not fully functional
Press RETURN to continue 
commit af334669319406775f87c8f54cc3e22df09a4f8c (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Jun 11 21:29:56 2024 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: `4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7`
+- password: xxxxxxxxxx
```

## Level 30
for this level i did all as b4, and did `git show <commit>` for 2 commits that had
and `git branch -a` then it will 
```
* dev
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

one of them has the password... dev or sploits-dev... 
`git checkout dev`
and `cat README.md`

```
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

## Level 31
same things, like b4 until cat README.md
well i dont see branches and logs so i will cheat and first thing i see is `git tags` which ive never used and was always curious so will paste from the blog, `Git tagging is a way to mark specific points in the history of the repository. One example would be to mark release points of the software. The command to see the tags is git tag. To create a tag the command is git tag -a <tag_name> -m <"tag description/message">. To see more details, like the tag message and commit, you can use the following command: git show <tag_name>.`

and now, still continuing to cheat since the above method similar to git branch -a might not work here?
`git tag` lists all tags and only `secret` was displayed
`git show secret`
got me the pass
(well it was just 2 cmds and i leart `git tags` today)
```
bandit30@bandit:/tmp/30/repo$ git tag
WARNING: terminal is not fully functional
Press RETURN to continue 
secret
bandit30@bandit:/tmp/30/repo$ git show secret
WARNING: terminal is not fully functional
Press RETURN to continue 
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
bandit30@bandit:/tmp/30/repo$ 
```

## Level 32
this one was ez as well but i used exhaustive list like tags, logs and branch -v b4 cating README and in the end readme had the answer

damn, i did it all right and got ` ! [remote rejected] master -> master (pre-receive hook declined)`

things done were, `echo "May I come in?" >> key.txt`
`git status` `ls` `git add .` status^ `rm .gitignore` add^ `git commit -m "lol"`
`git push` and i got above specified thing...

time to cheat and see what i did wrong maybe wasnt supposed to rm gitignore

well, well, imma retard.. i did it all right and redid it and got the same error but it has the passwd for next level but i only read the `red line`


```
bandit31-git@localhost's password: 
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 2 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 552 bytes | 552.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://localhost:2220/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://localhost:2220/home/bandit31-git/repo'
```

## Level 33
goal..
`After all this git stuff, it’s time for another escape. Good luck!`
out of git loop, they were too quick to solve,

cmds for this level is `sh, man`

you can read the description on linux variables [here](https://mayadevbe.me/posts/overthewire/bandit/level33/) and many other places and i dont want to write about them... i want to solve.. 

everything typed becomes UPPERCASE, so `$0` has ref to shell and is known by umm.. a type of linux users and so that was the solution... still i cheated

solution follows, by running shell and tying `bash` so you get the autocomplete back `cat /etc/bandit_pass/bandit33` tabbing and get password...

the reason this worked is bc if you do `ls -l` you will see the file uppercase is being run as bandit33 suid thing from prev. and so on..
```
bandit33@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit33 bandit32 15136 Jun 11 21:30 uppershell
```
also we are bandit33 and so we can read the file directly and even `whoami` will give away the current user as bandit33

```
bandit33@bandit:~$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

## Level 34 
you login user prev. and you get a readme 

that gives `congratulations`
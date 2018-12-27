<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Simple Servers and Server Abuse](#simple-servers-and-server-abuse)
  - [Web Server](#web-server)
    - [Perl Simple Webserver](#perl-simple-webserver)
    - [Python3 Simple Webserver](#python3-simple-webserver)
    - [Who Still Uses Python2](#who-still-uses-python2)
    - [Netcat Simple Server on Port 7000](#netcat-simple-server-on-port-7000)
    - [PHP Simple Webserver](#php-simple-webserver)
    - [Share A Single File](#share-a-single-file)
  - [Other Ways Of Accessing Files](#other-ways-of-accessing-files)
    - [Mount Folder or Filesystem Over SSH](#mount-folder-or-filesystem-over-ssh)
    - [Backup And Synchronize Entire Remote Folder Locally](#backup-and-synchronize-entire-remote-folder-locally)
    - [Move Lots Of Files Over SSH](#move-lots-of-files-over-ssh)
    - [The scp Version To Move Lots Of Files](#the-scp-version-to-move-lots-of-files)
    - [Push A Single File To Up To 32 Servers](#push-a-single-file-to-up-to-32-servers)
    - [Save and Stream A Remote File](#save-and-stream-a-remote-file)
  - [Server Access](#server-access)
    - [Copy Your SSH Public Key To A Server That Does Not Have ssh-copy-id](#copy-your-ssh-public-key-to-a-server-that-does-not-have-ssh-copy-id)
    - [Run A Command On A List Of Remote Servers Read From A File](#run-a-command-on-a-list-of-remote-servers-read-from-a-file)
    - [Run Local Script On Remote Server](#run-local-script-on-remote-server)
    - [Forward Your Private Keys Through SSH](#forward-your-private-keys-through-ssh)
  - [Server Abuse From The Command Line](#server-abuse-from-the-command-line)
    - [Get Futurama Quotes From slashdot.org](#get-futurama-quotes-from-slashdotorg)
    - [Stream Youtube URL Directly To mplayer](#stream-youtube-url-directly-to-mplayer)
    - [Simple Multi User Encrypted Chat Server For 5 Users](#simple-multi-user-encrypted-chat-server-for-5-users)
    - [Running GUI Programs Remotely](#running-gui-programs-remotely)
    - [BOFH Excuse Server](#bofh-excuse-server)
    - [A Simple Linux Remote Desktop](#a-simple-linux-remote-desktop)
    - [Find The Fastest Free DNS](#find-the-fastest-free-dns)
  - [Fix Problems You Never Knew You Had](#fix-problems-you-never-knew-you-had)
    - [Identify Differences Between Directories](#identify-differences-between-directories)
    - [Record Output Of Any Command From Server](#record-output-of-any-command-from-server)
    - [Pack Up Local Files Into A Tarball On Remote System Without Writing Local](#pack-up-local-files-into-a-tarball-on-remote-system-without-writing-local)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Simple Servers and Server Abuse

## Web Server

More specifically, these are simple file servers, though they can act as
static web servers with the inclusion of an index.html file.  Included are
simple servers made to direct specifically to an index.html or other file.

### Perl Simple Webserver
As perl again abuses the term 'simple'.
```
perl -MIO::All -e 'io(":8080")->fork->accept->(sub { $_[0] < io(-x $1 ? "./$1 |" : $1) if /^GET \/(.*) / })'
```

### Python3 Simple Webserver
```
python -m http.server
```

### Who Still Uses Python2
```
python -m SimpleHTTPServer
```

### Netcat Simple Server on Port 7000
```
while true; do nc -l 7000 | tar -xvf -; done
```
also, there is
```
while true ; do nc -l 7000 < index.html ; done
```

### PHP Simple Webserver
```
php -S 127.0.0.1:8080
```

### Share A Single File
```
nc -v -l 80 < file.ext
```

## Other Ways Of Accessing Files

### Mount Folder or Filesystem Over SSH
```
sshfs name@server:/path/to/folder /path/to/mount/point
```

### Backup And Synchronize Entire Remote Folder Locally
This needs curlftps
```
curlftpfs ftp://YourUsername:YourPassword@YourFTPServerURL /tmp/remote-website/ && rsync -av /tmp/remote-website/* /usr/local/data_latest && umount /tmp/remote-website
```

### Move Lots Of Files Over SSH
```
rsync -az /home/user/test user@sshServer:/tmp/
```
Meanwhile this will create an exact mirror on a remote server
```
rsync -e "/usr/bin/sss -p22" -a --progress --stats --delete -l -z -v -r -p /root/files/ user@remote_server:/root/files/
```

### The scp Version To Move Lots Of Files
```
tar czv file1 file2 folder1 | ssh user@server tar zxv -C /destination
```

### Push A Single File To Up To 32 Servers
This requires pssh
```
pscp -h hosts.txt -l username /etc/hosts /tmp/hosts<Paste>
```

### Save and Stream A Remote File
```
ssh USER@HOST cat REMOTE_FILE.mp4 | tee LOCAL_FILE.mp4 | mplayer -
```

## Server Access

### Copy Your SSH Public Key To A Server That Does Not Have ssh-copy-id
```
cat ~/.ssh/id_rsa.pub | ssh user@machine "mkdir ~/.ssh; cat >>
~/.ssh/authorized_keys"
```

### Run A Command On A List Of Remote Servers Read From A File
```
while read server; do ssh -n user@$server "command"; done < servers.txt
```

### Run Local Script On Remote Server
```
ssh -T user@server < script.sh
```

### Forward Your Private Keys Through SSH
Useful when logging into SSH, and then logging into another remote through
that.  Uses the -A flag for this purpose.
```
ssh -A user@somehost
```

## Server Abuse From The Command Line

### Get Futurama Quotes From slashdot.org
An alternative to using a fortune file
```
echo -e "HEAD / HTTP/1.1\nHost: slashdot.org\n\n" | nc slashdot.org 80 | egrep "Bender|Fry" | sed "s/X-//"
```

### Stream Youtube URL Directly To mplayer
Where 'i' is video-id
```
i="8uyxVmdaJ-w";mplayer -fs $(curl -s
"http://www.youtube.com/get_video_info?&video_id=$i" | echo -e $(sed
's/%/\\x/g;s/.*\(v[0-9]\.lscache.*\)/http:\/\/\1/g') | grep -oP '^[^|,]*')
```
I have the excellent `youtube-viewer` mapped to `YT` which will play the URL given as an argument.

### Simple Multi User Encrypted Chat Server For 5 Users
```
ncat -vlm 5 --ssl --chat 9876
```

### Running GUI Programs Remotely
Requires X11Forwarding
```
ssh -fX <user>@<host> <program>
```

### BOFH Excuse Server
```
telnet towel.blinkenlights.nl 666
```

### A Simple Linux Remote Desktop
Not a one liner, but interesting enough to be considered a recipe.
First run
```
X:12.0 vt12 2>&1 >/dev/null &
```
This opens an X-session on your 12th console.  While still on your main screen,
do
```
xterm -display :12.0 -e ssh -X user@server &
```
Now go to your 12th console (<Ctrl>+<Alt>+<F12>) and login!

### Find The Fastest Free DNS
There are more now, so this could be updated.  It is in the form of a function.
```
timeDNS() { parallel -j0 --tag dig @{} "$*" ::: 208.67.222.222 208.67.220.220 198.153.192.1 198.153.194.1 156.154.70.1 156.154.71.1 8.8.8.8 8.8.4.4 | grep Query | sort -nk5; }
```

## Fix Problems You Never Knew You Had

These are the things you simply do not think about.

### Identify Differences Between Directories
The example shows how to do this on two different servers, but it can be done with local as well.
```
diff <(ssh server01 'cd config; find . -type f -exec md5sum {} \;| sort -k 2')
<(ssh server02 'cd config;find . -type f -exec md5sum {} \;| sort -k 2')
```

### Record Output Of Any Command From Server
```
ssh user@server | tee <command>
```
Or you can just look at everything with a function
```
myssh() { ssh $1 | tee sshlog ; }
```

### Pack Up Local Files Into A Tarball On Remote System Without Writing Local
Interesting case here.  If you need to send a lot of files and you cannot
actually write to the local filesystem, this will pipe it through to the
remote.
```
tar -czf - * | ssh example.com "cat > files.tar.gz"
```


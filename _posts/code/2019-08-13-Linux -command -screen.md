---
layout: blog
comments: true
code: true
title:  "Screen command"
tags:
- Linux
- Screen
background-image: https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg
date:   2019-08-13 10:27:00
category: code
---

## Linux command **screen**

**Screen** is a full-screen software program that can be used to multiplexes a physical console between several processes (typically interactive shells). It offers a user to open several separate terminal instances inside a one single terminal window manager.

用中文说，就是screen提供了用户在一个物理console之上并行多个进程的接口。

screen目前我使用到的功能：支持后台部署应用。

### Installation

ubuntu

```
apt-get install screen
```

centos

```
yum install screen
```

### Usage

```
screen
```

```
Screen key bindings, page 1 of 1.
Command key:  ^A   Literal ^A:  a
 break       ^B b         flow        ^F f         
 lockscreen  ^X x         pow_break   B            
 screen      ^C c         width       W
  clear       C            focus       ^I           
  log         H            pow_detach  D            
  select      '            windows     ^W w
  colon       :            hardcopy    h            
  login       L            prev        ^H ^P p ^?   
  silence     _            wrap        ^R r
  copy        ^[ [         help        ?            
  meta        a            quit        \            
  split       S            writebuf    >
  detach      ^D d         history     { }          
  monitor     M            readbuf     <            
  suspend     ^Z z         xoff        ^S s
  digraph     ^V           info        i            
  next        ^@ ^N sp n   redisplay   ^L l         
  time        ^T t         xon         ^Q q
  displays    *            kill        K k          
  number      N            remove      X            
  title       A
  dumptermcap .            lastmsg     ^M m         
  only        Q            removebuf   =            
  vbell       ^G
  fit         F            license     ,            
  other       ^A           reset       Z            
  version     v
  ^]  paste .
"   windowlist -b
-   select -
0   select 0
1   select 1
2   select 2
3   select 3
4   select 4
5   select 5
6   select 6
7   select 7
8   select 8
9   select 9
I   login on
O   login off
]   paste .
```

### Example

Take a look at this command. First, you have to enter the screen.

```
pungki@mint ~ $ screen
```

Then you can do the download process. For examples on my Linux Mint, I am upgrading my **dpkg** package using **apt-get** command.

```
pungki@mint ~ $ sudo apt-get install dpkg
```

##### Sample Output

```
Reading package lists... Done
Building dependency tree      
Reading state information... Done
The following packages will be upgraded:
  dpkg
1 upgraded, 0 newly installed, 0 to remove and 1146 not upgraded.
Need to get 2,583 kB of archives.
After this operation, 127 kB of additional disk space will be used.
Get:1 http://debian.linuxmint.com/latest/ testing/main dpkg i386 1.16.10 [2,583 kB]
47% [1 dpkg 1,625 kB/2,583 kB 47%]                                        14,7 kB/s
```

While downloading in progress, you can press “**Ctrl-A**” and “**d**“. You will not see anything when you press those buttons. The output will be like this:

```
[detached from 5561.pts-0.mint]
pungki@mint ~ $
```

### Re-attach the screen

After you detach the screen, let say you are disconnecting your **SSH** session and going home. In your home, you start to **SSH** again to your server and you want to see the progress of your download process. To do that, you need to restore the screen. You can run this command:

```
pungki@mint ~ $ screen -r
```

And you will see that the process you left is still running.

When you have more than **1 screen** session, you need to type the screen session **ID**. Use screen **-ls** to see how many screen are available.

```
pungki@mint ~ $ screen -ls
```

##### Sample Output

```
pungki@mint ~ $ screen -ls
There are screens on:
        7849.pts-0.mint (10/06/2013 01:50:45 PM)        (Detached)
        5561.pts-0.mint (10/06/2013 11:12:05 AM)        (Detached)
2 Sockets in /var/run/screen/S-pungki
```

If you want to restore screen **7849.pts-0**.mint, then type this command.

```
pungki@mint ~ $ screen -r 7849
```

### Using Multiple Screen

When you need more than **1 screen** to do your job, is it possible? Yes it is. You can run multiple screen window at the same time. There are 2 (two) ways to do it.

First, you can detach the first screen and the run another screen on the real terminal. Second, you do nested screen.

### Switching between screens

When you do nested screen, you can switch between screen using command “**Ctrl-A**” and “**n**“. It will be move to the next screen. When you need to go to the previous screen, just press “**Ctrl-A**” and “**p**“.

To create a new screen window, just press “**Ctrl-A**” and “**c**“.

### Logging whatever you do

Sometimes it is important to **record** what you have done while you are in the console. Let say you are a **Linux Administrator** who manage a lot of Linux servers.

With this screen logging, you don’t need to write down every single command that you have done. To activate screen logging function, just press “**Ctrl-A**” and “**H**“. (Please be careful, we use capital ‘**H**’ letter. Using non capital ‘**h**’, will only create a screenshot of screen in another file named hardcopy).

At the bottom left of the screen, there will be a notification that tells you like: Creating logfile “**screenlog.0**“. You will find **screenlog.0** file in your home directory.

This feature will append everything you do while you are in the screen window. To close screen to log running activity, press “**Ctrl-A**” and “**H**” again.

Another way to activate logging feature, you can add the parameter “**-L**” when the first time running screen. The command will be like this.

```
pungki@mint ~ $ screen -L
```

### Lock screen

Screen also have shortcut to **lock** the screen. You can press “**Ctrl-A**” and “**x**” shortcut to lock the screen. This is handy if you want to lock your screen quickly. Here’s a sample output of lock screen after you press the shortcut.

```
Screen used by Pungki Arianto  on mint.
Password:
```

You can use your Linux password to unlock it.

### Add password to lock screen

For security reason, you may want to put the **password** to your screen session. A Password will be asked whenever you want to **re-attach** the screen. This password is different with **Lock Screen** mechanism above.

To make your screen password protected, you can edit “**$HOME/.screenrc**” file. If the file doesn’t exist, you can create it manually. The syntax will be like this.

```
password crypt_password
```

To create “**crypt_password**” above, you can use “**mkpasswd**” command on Linux. Here’s the command with password “**pungki123**“.

```
pungki@mint ~ $ mkpasswd pungki123
l2BIBzvIeQNOs
```

**mkpasswd** will generate a hash password as shown above. Once you get the hash password, you can copy it into your “**.screenrc**” file and save it. So the “**.screenrc**” file will be like this.

```
password l2BIBzvIeQNOs
```

Next time you run screen and detach it, password will be asked when you try to **re-attach** it, as shown below:

```
pungki@mint ~ $ screen -r 5741
Screen password:
```

Type your password, which is “**pungki123**” and the screen will **re-attach** again.

After you implement this screen password and you press “**Ctrl-A**” and “**x**” , then the output will be like this.

```
Screen used by Pungki Arianto  on mint.
Password:
Screen password:
```

A Password will be asked to you **twice**. First password is your **Linux password**, and the second password is the password that you put in your **.screenrc** file.

### Leaving Screen

There are **2** (two) ways to leaving the screen. First, we are using “**Ctrl-A**” and “**d**” to detach the screen. Second, we can use the exit command to terminating screen. You also can use “**Ctrl-A**” and “**K**” to kill the screen.

That’s some of screen usage on daily basis. There are still a lot of features inside the **screen command**. You may see **screen man page** for more detail.



## Reference

[1]. <https://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/>
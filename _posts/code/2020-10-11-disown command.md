---
layout: blog
comments: true
code: true
title:  "Disown Command Usage"
tags:
- linux
- background running

background-image: https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg
date:   2020-10-11 23:35:00
category: code

---

Programs in linux can be run as daemon by using &. But sometimes after the shell is closed, the process is also killed.

Here comes the usage of disown.

- In the Unix shells ksh, bash, fish and zsh, the disown builtin command is used to remove jobs from the job table, or to mark jobs so that a SIGHUP signal is not sent to them if the parent shell receives it (e.g. if the user logs out).



So when shell is closed, the process will not received the SIGHUP signal, which means they are kept alive.

For example:

```
./bin/kibana & disown
```

Reference:

[1]. https://discuss.elastic.co/t/kibana-5601-connection-refused/103641

[2]. https://en.wikipedia.org/wiki/Disown_(Unix)#:~:text=In%20the%20Unix%20shells%20ksh,if%20the%20user%20logs%20out).
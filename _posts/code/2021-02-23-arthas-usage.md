---
layout: blog
comments: true
istop: false
code: true
title: "Online trouble-shooting with Arthas (1)"
background-image: http://arthas.gitee.io/_images/arthas.png
date:  2021-02-23 17:30:00
category: code
tags:
- java
- arthas
- trouble-shooting
---

Arthas from Alibaba, is an online trouble-shooting tool. 

> No JVM restart, no additional code changes. Arthas works as an observer, that is, it will never suspend your running threads.



Launching Arthas:

```sh
$ java -jar arthas-boot.jar
```

Then select the application to be monitored.

![](https://i.loli.net/2021/02/23/DARcyVKxPnjtbCU.png)

Stop Arthas:

```sh
$ quit
```

or

```sh
$ exit
```





Basic Commands: 

1. dashboard command

```sh
$ dashboard
```

>  The `dashboard` command allows you to view the real-time data panel of the current system.

<img src="https://i.loli.net/2021/02/23/hbzRfBm7XjClanv.png"/>



2. thread command

```sh
$ thread 1
```

> The `thread 1` command prints the stack of thread ID 1.
>
> Arthas supports pipes, and you can find `main class` with `thread 1 | grep 'main('`.

![](https://i.loli.net/2021/02/23/z2xICjcVpoGyUDO.png)



3. sc command

```sh
$ sc -d *MathGame
```

> The `sc` command can be used to find the loaded classes in the JVM

![](https://i.loli.net/2021/02/23/UKQ7VRMtsfx85p9.png)



4. jad command

```sh
$ jad demo.MathGame
```

> The `jad` command can be used to decompile the byte code

![](https://i.loli.net/2021/02/23/GUtzbRJu1VvSA7r.png)



5. watch command

```
$ watch demo.MathGame primeFactors returnObj
```

> The `watch` command can view the parameter/return value/exception of the method.
>
> `params` refers to parameters
>
> `-x 2` means the print depth is 2

![](https://i.loli.net/2021/02/23/yDq8MkFSIgHJ9u7.png)



### Reference

[1]. https://arthas.aliyun.com/doc/arthas-tutorials.html?language=cn
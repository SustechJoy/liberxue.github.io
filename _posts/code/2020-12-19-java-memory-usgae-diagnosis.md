---
layout: blog
comments: true
istop: false
code: true
title: "Common Commands for Monitoring Java Memory Usage"
background-image: https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/db1601e0-5f67-4d76-a6d1-3d5e5f2ca2d7/File/0d6e6e95bd1dab36ae3e9a8ba1493af0/completablefuture_in_java_8_.png
date:  2020-12-19 17:14:00
category: code
tags:
- java
- jps
- jinfo
- jstat
- jmap
- jstack
---

## 1. jps

> usage: jps [-help]
>        jps [-q] [-mlvV] [<hostid>]
>
> Definitions:
>     <hostid>:      <hostname>[:<port>]
>
> -q: only include pID
>
> -l: list the whole name of the java class
>
> -v: include the parameters passed to JVM

```
$ jps
```

The jps command monitor the running java processes and return their pid(s).

```

$ jps
167257 Jps
381 Bootstrap

```



## 2. jinfo

> Usage:
>     jinfo [option] <pid>
>         (to connect to running process)
>     jinfo [option] <executable <core>
>         (to connect to a core file)
>     jinfo [option] [server_id@]<remote server IP or hostname>
>         (to connect to remote debug server)
>
> where <option> is one of:
>     -flag <name>         to print the value of the named VM flag
>     -flag [+|-]<name>    to enable or disable the named VM flag
>     -flag <name>=<value> to set the named VM flag to the given value
>     -flags               to print VM flags
>     -sysprops            to print Java system properties
>     <no option>          to print both of the above
>     -h | -help           to print this help message

```
$ jinfo 381
```

The jinfo returned both flags and sysprops

```
flags: vm info
sysprops: system properties
```



## 3. jstat

> Usage: jstat -help|-options
>        jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
>
> Definitions:
>   <option>      An option reported by the -options option
>   <vmid>        Virtual Machine Identifier. A vmid takes the following form:
>                      <lvmid>[@<hostname>[:<port>]]
>                 Where <lvmid> is the local vm identifier for the target
>                 Java virtual machine, typically a process id; <hostname> is
>                 the name of the host running the target Java virtual machine;
>                 and <port> is the port number for the rmiregistry on the
>                 target host. See the jvmstat documentation for a more complete
>                 description of the Virtual Machine Identifier.
>   <lines>       Number of samples between header lines.
>   <interval>    Sampling interval. The following forms are allowed:
>                     <n>["ms"|"s"]
>                 Where <n> is an integer and the suffix specifies the units as 
>                 milliseconds("ms") or seconds("s"). The default units are "ms".
>   <count>       Number of samples to take before terminating.
>   -J<flag>      Pass <flag> directly to the runtime system.

> Options:
>
> -class
> -compiler
> -gc
> -gccapacity
> -gccause
> -gcmetacapacity
> -gcnew
> -gcnewcapacity
> -gcold
> -gcoldcapacity
> -gcutil
> -printcompilation

- Loaded class info and costed memory.

```
$ jstat -class 379
```

```
Loaded  Bytes  Unloaded  Bytes     Time   
 36573 79849.3        0     0.0      96.54
```

- Real-time vm compiling info.

```
$ jstat -compiler 379
```

```
Compiled Failed Invalid   Time   FailedType FailedMethod
   59956     11       0   292.92          1 com/mysql/jdbc/AbandonedConnectionCleanupThread run
```

- GC info

```
$ jstat -gc 379
```

```
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT   
 0.0   12288.0  0.0   12288.0 1970176.0 342016.0 1163264.0   533809.0  293484.0 278813.9 27768.0 25483.1    265   11.824   0      0.000   11.824
```

The last five statistics indicates young GC count, young GC time span, full GC count, full GC time span, total GC time span.

```
jstat -gcutil 379
```

```
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT   
  0.00 100.00  27.88  45.90  95.00  91.77    266   11.849     0    0.000   11.849
```

- VM executing info.

```
$ jstat -printcompilation 379
```

```
Compiled  Size  Type Method
   59968    134    1 org/apache/skywalking/apm/dependencies/io/netty/util/concurrent/FastThreadLocal addToVariablesToRemove
```



## 4. jmap

> Usage:
>     jmap [option] <pid>
>         (to connect to running process)
>     jmap [option] <executable <core>
>         (to connect to a core file)
>     jmap [option] [server_id@]<remote server IP or hostname>
>         (to connect to remote debug server)
>
> where <option> is one of:
>     <none>               to print same info as Solaris pmap
>     -heap                to print java heap summary
>     -histo[:live]        to print histogram of java object heap; if the "live"
>                          suboption is specified, only count live objects
>     -clstats             to print class loader statistics
>     -finalizerinfo       to print information on objects awaiting finalization
>     -dump:<dump-options> to dump java heap in hprof binary format
>                          dump-options:
>                            live         dump only live objects; if not specified,
>                                         all objects in the heap are dumped.
>                            format=b     binary format
>                            file=<file>  dump heap to <file>
>                          Example: jmap -dump:live,format=b,file=heap.bin <pid>
>     -F                   force. Use with -dump:<dump-options> <pid> or -histo
>                          to force a heap dump or histogram when <pid> does not
>                          respond. The "live" suboption is not supported
>                          in this mode.
>     -h | -help           to print this help message
>     -J<flag>             to pass <flag> directly to the runtime system

- Shared object start address and other infos.

```
$ jmap 379
```

```
0x0000000000400000      8K      /usr/local/jdk1.8.0_192/bin/java
0x00007f44c81c4000      46K     /lib/x86_64-linux-gnu/libnss_files-2.19.so
0x00007f44eae18000      87K     /lib/x86_64-linux-gnu/libgcc_s.so.1
0x00007f44eb02e000      276K    /usr/local/jdk1.8.0_192/jre/lib/amd64/libsunec.so
0x00007f44ebced000      91K     /usr/local/jdk1.8.0_192/jre/lib/amd64/libnio.so
0x00007f450897e000      110K    /usr/local/jdk1.8.0_192/jre/lib/amd64/libnet.so
0x00007f4508b95000      50K     /usr/local/jdk1.8.0_192/jre/lib/amd64/libmanagement.so
0x00007f453bde4000      123K    /usr/local/jdk1.8.0_192/jre/lib/amd64/libzip.so
0x00007f4540128000      50K     /usr/local/jdk1.8.0_192/jre/lib/amd64/libinstrument.so
0x00007f4540333000      226K    /usr/local/jdk1.8.0_192/jre/lib/amd64/libjava.so
0x00007f4540562000      64K     /usr/local/jdk1.8.0_192/jre/lib/amd64/libverify.so
0x00007f4540771000      31K     /lib/x86_64-linux-gnu/librt-2.19.so
0x00007f4540979000      1026K   /lib/x86_64-linux-gnu/libm-2.19.so
0x00007f4540c7a000      16629K  /usr/local/jdk1.8.0_192/jre/lib/amd64/server/libjvm.so
0x00007f4541c5f000      1697K   /lib/x86_64-linux-gnu/libc-2.19.so
0x00007f454200a000      14K     /lib/x86_64-linux-gnu/libdl-2.19.so
0x00007f454220e000      106K    /usr/local/jdk1.8.0_192/lib/amd64/jli/libjli.so
0x00007f4542426000      134K    /lib/x86_64-linux-gnu/libpthread-2.19.so
0x00007f4542643000      137K    /lib/x86_64-linux-gnu/ld-2.19.so
```

- View the status of heap

```
$ jmap -heap 379
```

```
using thread-local object allocation.
Garbage-First (G1) GC with 2 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 3221225472 (3072.0MB)
   NewSize                  = 1363144 (1.2999954223632812MB)
   MaxNewSize               = 1932525568 (1843.0MB)
   OldSize                  = 5452592 (5.1999969482421875MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 1048576 (1.0MB)

Heap Usage:
G1 Heap:
   regions  = 3072
   capacity = 3221225472 (3072.0MB)
   used     = 829896752 (791.4512176513672MB)
   free     = 2391328720 (2280.548782348633MB)
   25.763385991255443% used
G1 Young Generation:
Eden Space:
   regions  = 256
   capacity = 2015363072 (1922.0MB)
   used     = 268435456 (256.0MB)
   free     = 1746927616 (1666.0MB)
   13.31945889698231% used
Survivor Space:
   regions  = 14
   capacity = 14680064 (14.0MB)
   used     = 14680064 (14.0MB)
   free     = 0 (0.0MB)
   100.0% used
G1 Old Generation:
   regions  = 524
   capacity = 1191182336 (1136.0MB)
   used     = 546781232 (521.4512176513672MB)
   free     = 644401104 (614.5487823486328MB)
   45.90239592001472% used
```

- View the objects count and memory usage in heap

```
$ jmap -histo 379
```

```
 num     #instances         #bytes  class name
----------------------------------------------
   1:       4034316      436696424  [C
   2:       2250930      182741120  [B
   3:       1650852       83011744  [Ljava.lang.Object;
   4:        313513       64386528  [I
   5:       2546267       61110408  java.lang.String
   6:        879073       42195504  java.util.HashMap
   7:        378719       33327272  java.lang.reflect.Method
   8:        488943       23324496  [Ljava.lang.String;
   9:        898825       21571800  java.util.ArrayList
  10:        375343       21019208  sun.nio.cs.UTF_8$Encoder
  11:        594213       19014816  java.util.concurrent.ConcurrentHashMap$Node
  12:        426986       13663552  java.util.HashMap$Node
  13:        281695       13521360  org.aspectj.weaver.reflect.ShadowMatchImpl
  14:        278037       13345776  sun.nio.cs.US_ASCII$Decoder
  15:        496650       11919600  java.lang.StringBuilder
  16:        247381        9591312  [[B
  17:        281695        9014240  org.aspectj.weaver.patterns.ExposedState
```

```
$ jmap -histo:live 379
```

The above triggers a full gc and counts only alive objects.

- Generate heap dump file

```
$ jmap -dump:format=b,file=test.hprof 379
```



## 5. jstack

> Usage:
>     jstack [-l] <pid>
>         (to connect to running process)
>     jstack -F [-m] [-l] <pid>
>         (to connect to a hung process)
>     jstack [-m] [-l] <executable> <core>
>         (to connect to a core file)
>     jstack [-m] [-l] [server_id@]<remote server IP or hostname>
>         (to connect to a remote debug server)
>
> Options:
>     -F  to force a thread dump. Use when jstack <pid> does not respond (process is hung)
>     -m  to print both java and native frames (mixed mode)
>     -l  long listing. Prints additional information about locks
>     -h or -help to print this help message

```
$ jstack 379 | less
```

```
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.192-b12 mixed mode):

"grpc-default-executor-376" #8349 daemon prio=5 os_prio=0 tid=0x00007f44ec43b000 nid=0x4a33f waiting on condition [0x00007f44c8688000]
   java.lang.Thread.State: TIMED_WAITING (parking)
        at sun.misc.Unsafe.park(Native Method)
        - parking to wait for  <0x000000070050cd20> (a java.util.concurrent.SynchronousQueue$TransferStack)
        at java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
        at java.util.concurrent.SynchronousQueue$TransferStack.awaitFulfill(SynchronousQueue.java:460)
        at java.util.concurrent.SynchronousQueue$TransferStack.transfer(SynchronousQueue.java:362)
        at java.util.concurrent.SynchronousQueue.poll(SynchronousQueue.java:941)
        at java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1073)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1134)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
        at java.lang.Thread.run(Thread.java:748)
...
```

```
$ top 
```

shift + H give out tID

```
Threads: 380 total,   1 running, 379 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.2 us,  0.0 sy,  0.0 ni, 94.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   5242880 total,  5220180 used,    22700 free,        0 buffers
KiB Swap:        0 total,        0 used,        0 free.  1567744 cached Mem

    PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                                                                                                         
    938 root      20   0 6333236 3.339g     32 S  3.0 66.8  35:02.07 java                                                                                                                            
    876 root      20   0 6333236 3.339g     32 S  2.7 66.8  29:38.96 java                                                                                                                            
    982 root      20   0 6333236 3.339g     32 S  0.7 66.8   3:32.86 java                                                                                                                            
 293281 root      20   0 6333236 3.339g     32 S  0.7 66.8   0:06.95 java 
```

We can view that the 938 thread cost the most CPU and last for more than 0.5h.

convert 938 to hex: 3aa

```
$ jstack {pID} | grep `printf "%x" {tID}`
```

```
"XXXThread-3" #118 daemon prio=5 os_prio=0 tid=0x00000000041ad000 nid=0x3d8 in Object.wait() [0x00007f44d3aa1000]
"YYYThread-4" #75 daemon prio=5 os_prio=0 tid=0x00007f44f4166000 nid=0x3aa runnable [0x00007f44d634f000]
```

So the **XXXThread-3** cost most CPU iterations and should be treated carefully.



### Reference:

[1]. https://www.cnblogs.com/sxFu/p/11751934.html

[2]. https://blog.csdn.net/chenxiusheng/article/details/74007040
---
layout: blog
comments: true
istop: false
code: true
title: "Partly UTF-8 Garbled Issue"
background-image: https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/db1601e0-5f67-4d76-a6d1-3d5e5f2ca2d7/File/0d6e6e95bd1dab36ae3e9a8ba1493af0/completablefuture_in_java_8_.png
date:  2021-01-01 23:30:00
category: code
tags:
- java
- garbled
- spring boot
- encoding
- UTF-8
---

When I was deploying my service to the server today, which is a debian 9 linux node, I met a strange issue.
The service is made with spring boot framework, running with jvm.

The response of api when I running locally:

```
{
    "statusCode": "200",
    "statusMessage": "成功!",
    "data": {
        "classId": 1,
        "className": "三年二班",
        "maxRoom": 1.0000000000
    },
    "message": null,
    "timeStamp": 1609514106583
}
```

But the server returns

```
{
    "statusCode": "200",
    "statusMessage": "����!",
    "data": {
        "classId": 1,
        "className": "三年二班",
        "maxRoom": 1.0000000000
    },
    "message": null,
    "timeStamp": 1609514106583
}
```

The statusMessage is garbled. However, the `data.className` keeps a normal behavior.



As the response body is **partly** garbled, the issue can be speculated as the java file was not compiled with encoding utf-8.

try add

```
-Dfile.encoding=UTF-8
```

to the end of /etc/profile.



Then 

```
mvn package
```



Finally, the response body return to normal.

```
"statusMessage": "成功!",
```


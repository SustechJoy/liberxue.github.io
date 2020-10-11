---
layout: blog
comments: true
code: true
title:  "Enviornmental Problem When Installing Applications Under 'sudo' Mode"
tags:
- linux
- environment setting
- sudo

background-image: https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg
date:   2020-10-11 21:45:00
category: code

---

I have encountered the same problem twice until I met a simple solution to the following problem tonight:

```
could not find java, set java_home
```

which is caused by enviornment variables are reset when getting into the super user mode.



To keep your enviorment variables. use ```-E``` after sudo, which helps to maintain the path variables even enter the super user mode. For example,

```
sudo -E  apt-get install ./elasticsearch-6.8.1.deb
```

Here's more detail about ```-E```

```
  -E   The -E (preserve environment) option indicates to the security policy that the user wishes
       to preserve their existing environment variables.  The security policy may return an error 
       if the -E option is specified and the user does not have permission to preserve the environment
```



Reference:

[1]. https://stackoverflow.com/questions/12309253/sudo-java-command-not-found-after-exiting-from-root-user


---
layout: post
title: Docker is Not Native
---


Starting June 20, 2016 docker for Mac and Windows are now available as a public beta. The new docker runs natively on Mac and Windows. Those of us who have dealt with docker-machine for any amount of time know how painful it was to deal with. I mostly blame VirtualBox. Nevertheless, the new versions of docker do not rely on docker-machine anymore. You can run docker commands without messing with your environment or VirtualBox.

Unfortunately this new release claiming "native" execution is confusing some people. Many believe that docker is now completely cross platform, but it is not. The technology behind docker relies *entirely* on the Linux kernel. Running docker without Linux is as feasible as driving a 1967 Chevrolet Camaro without an engine. It is not yet possible. Containerization is a kernel-level virtualization mechanism which allows you to segregate individual parts of the kernel itself. You can create contained networks, process trees, users, groups, file systems, etc. This allows docker to create individual "environments" for each running container. These things simply are not possible with the current Windows or Mac kernels.

Then how does docker run on Mac and Windows "natively?" The answer is similar to how they ran before, but *much* better. The docker tools create a *native* virtual operating system on the host machine. On Mac, they use [xhyve](https://github.com/mist64/xhyve) which is built on top of the OSX Hypervisor. With this they create an [Alpine **Linux**](http://www.alpinelinux.org/) virtual machine. All of your docker commands run seamlessly against the VM. If you are running the latest version of docker (1.12.0-rc3 or later) then run `docker info` in the terminal. You are going to see something similar to the following.

```
Kernel Version: 4.4.14-moby
Operating System: Alpine Linux v3.4
OSType: linux
Architecture: x86_64
CPUs: 4
Total Memory: 1.954 GiB
```

This is how you can open the docker preferences and modify settings like CPUs, and Memory. You are not making changes to docker, you are making changes to the VM which is running Alpine Linux, which is running docker.

I do not mean to tear down anything docker has done. What they have accomplished is amazing. The new native versions of docker are exponentially better than the last, and even in the RC stage I rate them 10/10. Docker is something that has changed my development life in so many ways. That being said, docker *cannot* currently be run in on anything but Linux. The new Docker Tools just remove the VM management hassle and manage it all behind the scenes and it does a damn good job at it. It is doing it so well that it has made people believe that it is running without Linux. 

# Namespaces, Linux, and Docker Security

## Summary
When creating a new Docker image, it runs all commands within as 'root.' This can be used to 'breakout' of the container and access the host (see references). To mitigate this security flaw, Docker has instituted use of namespaces. These notes attempt to make sense of what that is and how to use it.

**TL;DR**
From the [Docker Security Page](https://docs.docker.com/engine/security/security/):
> Docker containers are, by default, quite secure; especially if you take care of running your processes inside the containers as non-privileged users (i.e., non-root).

Doing this is considered an advanced feature and results in some restrictions for how you use your container. See below section "Known restrictions when using namespaces."

## Resources
[Docker Security Page](https://docs.docker.com/engine/security/security/)
[Docker daemon command reference](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-user-namespace-options)
[Implementation of User Namespaces in Docker](https://integratedcode.us/2015/10/13/user-namespaces-have-arrived-in-docker/)
[Creating Containers](http://crosbymichael.com/creating-containers-part-1.html)
[Expanation of how escaping works](http://container-solutions.com/docker-security-admin-controls-2/)
[Stack Exchange post](http://security.stackexchange.com/questions/106860/can-a-root-user-inside-a-docker-lxc-break-the-security-of-the-whole-system)
[Isolating Your System with Linux Namespaces](https://www.toptal.com/linux/separation-anxiety-isolating-your-system-with-linux-namespaces)
[Current Solutions for Docker Security](http://blogs.oregonstate.edu/developer/2016/03/28/current-solutions-for-docker-security/) (03/2016)
[Introduction to User Namespaces in Docker Engine](https://success.docker.com/Datacenter/Apply/Introduction_to_User_Namespaces_in_Docker_Engine)
[Ubuntu User Management](https://help.ubuntu.com/lts/serverguide/user-management.html)
[Linux Add User Command](https://www.pluralsight.com/blog/tutorials/linux-add-user-command)
[Add User to Docker Container](http://stackoverflow.com/questions/27701930/add-user-to-docker-container) (Stack Overflow)
[How to run a more secure non-root user container](https://www.projectatomic.io/blog/2016/01/how-to-run-a-more-secure-non-root-user-container/)
[Docker image author guidance](http://www.projectatomic.io/docs/docker-image-author-guidance/)


## [Docker Security Page](https://docs.docker.com/engine/security/security/)
As of Docker 1.10 User Namespaces are supported directly by the docker daemon. This feature allows for the root user in a container to be mapped to a non uid-0 user outside the container, which can help to mitigate the risks of container breakout. This facility is available but not enabled by default.

## [Docker Daemon Command Reference](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-user-namespace-options)
**Disabling user namespace for a container**
If you enable user namespaces on the daemon, all containers are started with user namespaces enabled. In some situations you might want to disable this feature for a container, for example, to start a privileged container. To enable those advanced features for a specific container, use `--userns=host` in the `run/exec/create` command. This option will completely disable user namespace mapping for the container's user. 

**Known restrictions when using namespaces**
The following standard Docker features are currently incompatible when running a Docker daemon with user namespaces enabled:
* sharing PID or NET namespaces with the host, i.e. `--pid=host` or `--network=host`
* a `--read-only` container filesystem (this is a Linux kernel restriction against remounting with modified flags of a currently mounted filesystem when inside a user namespace)
* external (volume or graph) drivers which are unaware/incapable of using daemon user mappings
* using `--privileged` mode flag on `docker run` (unless  also specifying `--userns=host`

In general, user namespaces are an advanced feature and will require coordination with other capabilities. For example, if volumes are mounted from the host, file ownership will have to be pre-arranged if the user or administrator wishes the containers to have expected access to the volume contents.

## [Creating Containers](http://crosbymichael.com/creating-containers-part-1.html) provides good overview of namespaces in general
Ironically, he uses a container to demonstrate how namespaces work. The only reason it's a container is because he dependencies needed to run the examples. Here's the container and run command to spin it up: 
`docker run -it --privileged --net host crosbymichael/make-containers`

Note that he's using the `--privileged` flag when running that container. From the Docker docs:
> By default, Docker containers are “unprivileged” and cannot, for example, run a Docker daemon inside a Docker container. This is because by default a container is not allowed to access any devices, but a “privileged” container is given access to all devices (see the documentation on cgroups devices). When the operator executes docker run --privileged, Docker will enable to access to all devices on the host as well as set some configuration in AppArmor or SELinux to allow the container nearly all the same access to the host as processes running outside containers on the host. Additional information about running with --privileged is available on the [Docker Blog](http://blog.docker.com/2013/09/docker-can-now-run-within-docker/).

The examples are too complicated for me to understand at the time of this writing but it was worth it for the flag info.

## [Isolating Your System with Linux Namespaces](https://www.toptal.com/linux/separation-anxiety-isolating-your-system-with-linux-namespaces)
Docker containers are so much lighter than full VMs because it uses some of your existing (host) system, including namespaces.

**Process Namespace**
Historically, the Linux kernel has maintained a single process tree. The tree contains a reference to every process currently running in a parent-child hierarchy. A process, given it has sufficient privileges and satisfies certain conditions, can inspect another process by attaching a tracer to it or may even be able to kill it.

With the introduction of Linux namespaces, it became possible to have multiple “nested” process trees. Each process tree can have an entirely isolated set of processes. This can ensure that processes belonging to one process tree cannot inspect or kill - in fact cannot even know of the existence of - processes in other sibling or parent process trees.

Every time a computer with Linux boots up, it starts with just one process, with process identifier (PID) 1. This process is the root of the process tree, and it initiates the rest of the system by performing the appropriate maintenance work and starting the correct daemons/services. All the other processes start below this process in the tree. The PID namespace allows one to spin off a new tree, with its own PID 1 process. The process that does this remains in the parent namespace, in the original tree, but makes the child the root of its own process tree.

With PID namespace isolation, processes in the child namespace have no way of knowing of the parent process’s existence. However, processes in the parent namespace have a complete view of processes in the child namespace, as if they were any other process in the parent namespace.

![process tree](https://uploads.toptal.io/blog/image/674/toptal-blog-image-1416487554032.png)

##[Implementation of User Namespaces in Docker](https://integratedcode.us/2015/10/13/user-namespaces-have-arrived-in-docker/)
One of the most important features of user namespaces is that it allows containers to have a different view of the uid and gid ranges than the host system.  Specifically, a process (and in our case, the process(es) inside our container) can be provided a set of mappings from the host uid and gid space, such that when the process thinks it is running as uid 0 (commonly known as “root”), it may actually be running as uid 1000, or 10000, or even 34934322.  It all depends on the mappings we provide when we create the process inside a user namespace. Of course, it should be clear that from a security perspective this is a great feature as it allows our containers to continue running with root privileges, but without actually having any root privilege on the host.

## [Current Solutions for Docker Security](http://blogs.oregonstate.edu/developer/2016/03/28/current-solutions-for-docker-security/) (03/2016)
Despite the namespacing fix by Docker being announced in early 2016, this guy is writing as if it's still a problem. Hmm.

His solution to the breakout problem includes strategies of "least privilege":
* do not run process in a container as a root to avoid root access from attackers.
* run filesystems as read-only so that attackers can not overwrite data or save malicious scripts to file
* limit the resources that a container can use

Docker utilizes PID namespace to separate container processes from the host as well as other containers, so that processes in a container can’t observe or do anything to the other processes running in the host or in other containers.

##[Introduction to User Namespaces in Docker Engine](https://success.docker.com/Datacenter/Apply/Introduction_to_User_Namespaces_in_Docker_Engine)


## Notes
Inside a container, you can check what user you're using by using the command `ps all`. '0' indicates root user. 
To see what users are available on a machine or container (Linux) run `cat /etc/passwd`
The format for the results are:
username:x:userid:groupid::homedirectory:shell
Examples:
`roman:x:1025:100::/home/roman:/bin/bash`
`root:x:0:0:root:/root:/bin/bash`
`x` indicates an encrypted password is stored in the /etc/shadow file
userid is sometimes seen as UID. groupid is sometimes seen as GID. PID = process id.
above examples are from [linux add user command](https://www.pluralsight.com/blog/tutorials/linux-add-user-command) post.
To add a user, 
`useradd mepler -m` where `-m` creates a directory with that user's name at `/home/username`
To switch to a user:
`su mepler` which will also change the pwd to `/home/username`
To delete a user:
`userdel -r mepler` where `-r` deletes the home directory of that user

## [Docker image author guidance](http://www.projectatomic.io/docs/docker-image-author-guidance/)
By default docker containers run as root. A docker container running as root has full control of the host system. As docker matures, more secure default options may become available. For now, requiring root is dangerous for others and may not be available in all environments. Your image should use the USER instruction to specify a non-root user for containers to run as. If your software does not create its own user, you can create a user and group in the Dockerfile as follows:
```
RUN groupadd -r swuser -g 433 && \
useradd -u 431 -r -g swuser -d <homedir> -s /sbin/nologin -c "Docker image user" swuser && \
chown -R swuser:swuser <homedir>
```

The default user in a Dockerfile is the user of the parent image. For example, if your image is derived from an image that uses a non-root user swuser, then RUN commands in your Dockerfile will run as swuser.

If you need to run as root, you should change the user to root at the beginning of your Dockerfile then change back to the correct user with another USER instruction:
```
USER root
RUN yum install -y <some package>
USER swuser
```

### --userns-remap
When user namespace support is enabled, container processes running as the 'root' user will have expected administrative privelege (with some restrictions) inside the container but will effectively be mapped to an unprivileged 'uid' on the host. 

To enable user namespace support, start the daemon with the `--userns-remap` flag, which accepts values in the following format:
* uid
* uid:gid
* username
* username:groupname

## Email from Rory McCune
Has a post on namespacing. Emailed him <raesene@gmail.com> about *when* to use namespacing. I liked his response:
> So user namespacing is an interesting one. I think that if you can be sure that all your containers will not run as root and also that you don’t have any risks from setUID binaries in the containers its possible to achieve a good level of security in that regard, without enabling user namespacing.
 
	My reasoning for recommending that its enabled is that it’s a “secure default”, so if you get a container from Docker Hub or similar which runs as root (and from what I’ve seen a lot of them do) or a container image has a vulnerable SetUID program, you’re not at risk.
 
	In a corporate environment where the admin may not have full control of all the images being run (e.g. if there are development teams using the docker host) then having those safer defaults seems like a good idea, where perhaps for a setup that the admin does have full control of what images will be run and also has the resources to audit them, it’s less of a concern.
	 
	In terms of resources, well there’s the CIS guide (https://benchmarks.cisecurity.org/tools2/docker/CIS_Docker_1.12.0_Benchmark_v1.0.0.pdf ) but it takes a very secure default position, which I guess isn’t surprising for a hardening guide.
	 
	Past that probably the best thing to do would be look at Phil Estes’ presentations on user namespacing.  He’s done quite a few talks on it…

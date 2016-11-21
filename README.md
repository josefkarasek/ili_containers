## ILI 2016 - Linux containers
This lab consist of several exercises in which students learn about linux containers. From basics like what is a linux container and how to get and run them to more advanced features, such as how to apply different kinds of linux namespaces and cgroups. Students don't interact with underlying kernel features directly, but rather use the docker tool, that makes working with linux containers a lot easier.

### Install the docker tool
Use the `yum` command to download the docker package and add your user to the `docker` group so you don't need to type `sudo` everytime you invoke the command.
```
sudo yum install docker
sudo groupadd docker
sudo gpasswd -a $(whoami) docker
sudo systemctl start docker
```
If you're wandering why this is necessary [this](http://www.projectatomic.io/blog/2015/08/why-we-dont-let-non-root-users-run-docker-in-centos-fedora-or-rhel/) article is for you.

Consult the manual page for the `run` subcommand: `man docker run`.

### The Lab
1) Hello World
```
docker run hello-world
```
2) First steps
```
docker run -i -t fedora:24 /bin/bash
pwd
ls
cat /etc/hostname
uname -r
cat /etc/fedora-release
ls -al /
```
Look around, anything interesting?
As the last command, delete something important...
```
rm -rf /usr/bin
ls
```
This system is broken! Let's get rid of it and start a new one:
```
exit
docker ps -a
docker rm <id>
docker run -i -t fedora bash
ls
exit
```
That's better, isn't it?

3) Namespaces
Install some basic tools that we'll use to gather information about the container at runtime:
```
docker run -it fedora
dnf install -y procps-ng iproute hostname
```
* Network namespace. Compare outputs of the following commands inside the container and on the host.
```
# in container:
ip a
# on the host:
ip a
```
* PID namespace
```
# in container:
ps ax
sleep 10000
# on the host:
ps axf | grep -v grep | grep -B 2 sleep
```
* UTS namespace
```
# in container:
hostname
# on the host:
hostname
```

3) CGroups

# Getting started with a development environment for the Statistical Working System

This readme includes:  

* Setting up the tools: vagrant, coreos, fleet and docker
* Getting ready with the environment

##Software stack
**Vagrant**:_"Vagrant is a tool for building complete development environments. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases development/production parity, and makes the "works on my machine" excuse a relic of the past."_ 

**Virtual Box**:_"VirtualBox is a general-purpose full virtualizer for x86 hardware, targeted at server, desktop and embedded use."_

**CoreOS**:_"CoreOS is a minimal operating system that supports popular container systems out of the box."_ Usually Linux distributions come with a lot of pre installed packages that your  software deployment will unlikely need. CoreOS strips Linux to the core and on top it comes with a pre installed Docker service.

**Docker**:_"Docker is an open-source project that automates the deployment of applications inside software containers, by providing an additional layer of abstraction and automation of operating-system-level virtualization on Linux."_

##MAC OS X
**Install the dependencies:**
*Vagrant* can be downloaded at the following [download](https://www.vagrantup.com/downloads.html) ULR. Here is the [link](https://dl.bintray.com/mitchellh/vagrant/vagrant_1.7.4.dmg) for the version this document is based on.
*Virtual Box* can be dowloaded at the following [download](https://www.virtualbox.org/wiki/Downloads) URL. Here is the [link](http://download.virtualbox.org/virtualbox/5.0.0/VirtualBox-5.0.0-101573-OSX.dmg) for the version this document is based on. Please note that version 4.3.10 or greater is required.

We need also a git in order to clone the configurations from github. However if you don't have it or you feel brave enough you can still download the raw files from the following [link](https://github.com/gsorbara/coreos-vagrant). While the repository is at the following URL `https://github.com/gsorbara/coreos-vagrant.git`

After installing the packages we should be able to create an CoreOS image on the local machine.
First let's check if everything has been installed correctly. In a terminal type `vagrant version`:

```
$ vagrant version
Installed Version: 1.7.4
Latest Version: 1.7.4
 
You're running an up-to-date version of Vagrant!
```

Now we need to clone the configuration file for the vagrant box. 
Inside a terminal 
```
$ cd to_your_preferred_folder
```

```
$ mkdir sws
$ cd sws
$ git clone https://github.com/gsorbara/coreos-vagrant.git
Cloning into 'coreos-vagrant'...
remote: Counting objects: 398, done.
remote: Total 398 (delta 0), reused 0 (delta 0), pack-reused 398
Receiving objects: 100% (398/398), 96.61 KiB | 0 bytes/s, done.
Resolving deltas: 100% (172/172), done.
Checking connectivity... done.
$ cd coreos-vagrant
```
The `user-data` file contains a list of configuration and services to be started at bootstrap time. The `user-data` downloaded from the git repository is configured to use `etcd`, `fleet` and `docker`. In order to properly configure `etcd` you should be requesting a new discovery key (this seems valid also for a single machine).

```
$ curl https://discovery.etcd.io/new?size=1 && echo
https://discovery.etcd.io/b89bc73bd2f4eeff619c921a474d59e7
$
```
Now you can edit the `user-data` file and under the `etcd:` configuration section replace `<your_key>` with the value returned by the service just called, in this case `b89bc73bd2f4eeff619c921a474d59e7`

...and we are ready to create our CoreOS machine.

```
$ vagrant up
```

At this point **Vagrant** will start interpreting the instruction and build the box containing **CoreOS**. After a while, depending on your internet connection, you should be able to see something like the following:

```
==> core-01: Box 'coreos-beta' could not be found. Attempting to find and install...
    core-01: Box Provider: virtualbox
    core-01: Box Version: >= 0
==> core-01: Loading metadata for box 'http://beta.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json'
    core-01: URL: http://beta.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json
...
a lot of other things here
...
==> core-01: Machine booted and ready!
==> core-01: Setting hostname...
==> core-01: Configuring and enabling network interfaces...
==> core-01: Running provisioner: file...
==> core-01: Running provisioner: shell...
    core-01: Running: inline script
```

At this point the machine is setup and we can connect via SSH

```
$ vagrant ssh
CoreOS beta (723.3.0)
core@core-01 ~ $ 
```




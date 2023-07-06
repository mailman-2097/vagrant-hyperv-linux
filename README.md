# Introduction

This project is an example of how to setup linux VM on Vagrant using Hyper V as the provider. 
My new workplace has AVD and the underlying VMs have nested virtualisation enabled.

The main motivation for this project is as follows:

1. Configure VM in HyperV using a Vagrantfile
1. Compare HyperV, VirtualBox and WSL

# HyperV vs ( VirtualBox || WSL )

## Pros of HyperV

1. Easy to configure HyperV with Powershell in Windows (if you have Windows professional license)
1. Native Windows solution so much faster than VirtualBox (In my case running virtual box was not an option)
1. The vagrant configuration is not that different
1. Shared file with SMB protocol
1. Vagrant ssh configuration works as expected 

## Cons of HyperV

1. Not all Vagrant Boxes are available as compared to other providers esp VirtualBox
1. WSL will be the better option in most cases
1. The experience is hit or miss depending on the Linux operating system and distribution 
1. Clipboard integration through the Hyper Console is really bad. 
1. File sharing between Host and Guest is over SMB protocol and requires domain credentials to be passed to VM. 
> I tried to setup a local admin user and pass those credentials; but it did not work.
> A domain credential is required. Have not tried this on my personal laptop

# Setup

1. Install HyperV

1. run 
```
vagrant up

```

# Final Thoughts
HyperV is definitely a good tool. 
But WSL may be the better choice for linux.
The only issue with WSL is that does not have proper Rhel clones except for Fedora Remix.
But VirtualBox works fine so if it ain't broke don't fix it!

# Known Issues

- RHEL clones may provide better results (ex: centos 9 stream)
- Ubuntu VMs did not get setup properly when using file shares :(
> Successful configuration of SMB shares are dependant on the underlying box supplier

- I experienced some Kernel bugs on VMs that were not under any load :(
```
Message from syslogd@centos9s at Jul  5 07:35:49 ...
 kernel:watchdog: BUG: soft lockup - CPU#1 stuck for 380s! [kworker/1:1:7656]

Message from syslogd@centos9s at Jul  5 07:36:29 ...
 kernel:watchdog: BUG: soft lockup - CPU#1 stuck for 417s! [kworker/1:1:7656]

Message from syslogd@centos9s at Jul  5 07:36:57 ...
 kernel:watchdog: BUG: soft lockup - CPU#1 stuck for 444s! [kworker/1:1:7656]

```
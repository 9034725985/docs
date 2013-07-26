---
title: SSH Into Your VM
author: leeolayvar
sidebar: {
  'Video': '#video',
  'What you will need': '#whatyouneed',
  'Tutorial steps': '#steps',
  'Confirming installation': '#confirminginstall',
  'Possible Gotchas': '#gotchas',
  'Additional Resources': '#additionalresources'
}
template: guide.jade
---


In this tutorial, we will go over the steps to SSHing into your VM! This
is a semi-experimental tutorial, so if you have any problems please make
sure to
[submit an issue](https://github.com/leeolayvar/koding-unofficial/issues/new)
and we can help make this a rock solid tutorial for all skill levels :)

Note that we will only be covering Unix based OSs. If someone wants to
give me information on how this can be done on Windows, i'll be happy
to add a windows section!

**Reminder**: As with all of these tutorials, they assume that there are no
conflicts. If you have previously attempted to install this software please
remove it fully or understand that conflicts may occur. Thanks :)



<a name="video"></a>
## Video

The following is an instructional video which approximately
mirrors the steps below. It covers both App, and Manual,
installations.

<iframe width="680" height="450" src="//www.youtube.com/embed/9tsmL61HgXU" frameborder="0" allowfullscreen></iframe>



<a name="whatyouneed"></a>
## What you will need

In this tutorial, i am going to use the terminology of "Home" and "Koding"
machines. Home being whatever the machine you are using to connect to
Koding with. Koding being the machine that is receiving the connection.
`Home -> Koding`.

On the home machine, you will need a public rsa key *(usually found
in `~/.ssh/id_rsa.pub`)*. If you need to generate one, please
[refer to the Github tutorial on this](https://help.github.com/articles/generating-ssh-keys).
You will also need your Username, and your VM Number.

To find your VM number, open your terminal for the VM you wish to ssh into,
and look for the *username@vm-0:~$*, where *vm-0*, *vm-1*,
*vm-2*, etc, is your vm number.

**Feedback**: If you feel Generating an SSH Key would be very helpful on this
tutorial, feel free to
[let me know](https://github.com/leeolayvar/koding-unofficial/issues/new) :)




<a name="steps"></a>
## Tutorial Steps

1. First, copy your Public Key, usually found in `~/.ssh/id_rsa.pub`, and copy
  it in it's entirety! It will look something like this:
  
  ```
  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCyhKankDE4DRM86JqZ3JPdWDeqg+TbzlqlTLf
  OKTeokhRoMgy5WoMY/ZWUVES3d2vSHHwW3cwWlELmVdc3Ow57boZv3fOsPhybYHVRTClXYr1ncS
  xyTvjvCfvV5q22aIxHPWQ353543ssda87sa+85XEa4VnveJsEzxBZl4oJ4GB0AGa48+UdIqutrg
  Zu7D7JCK+Yl228X+3bJf3ddlqDaKaVXPivvvYqImK6ZwFsxh2lNO4E8IOd3OSK9zv6i+io8PxWm
  wP0tLFokxulAI8Td1sOPBE9s9bdJ5c2T/GfGjKF+aNKsd33TsYEjjc/plMZmRRrOgQwre6OAkgM
  vyV2X foo@bar.baz
  ```
  
2. Next, paste this entire Public Key into your SSH Keys section of your
  Account settings. This can be found by going to
  [koding/Account](https://koding.com/Account) and clicking SSH Keys
  under the DEVELOP. Click the Plus button on the right side of the page,
  and paste your Public Key into this. Below is a screenshot of this area
  for clarification.
  ![Koding SSH Keys](sshkeys.png)
  
3. Now go back to your Home machine, and create the file
  `~/.ssh/config` *(assuming it's not already created)*. Add the following
  code into that file:
  
  ```
  Host *.kd.io
    User <username>
    ProxyCommand ssh %r@ssh.koding.com nc %h %p
  ```
  
  Where `<username>` is your username, without the `<>`.
  
4. Next, on your Home machine and connect to your VM! This can be
  done by typing: `ssh <vm-Number>.<username>.koding.kd.io`. An example,
  here is my connection command: `ssh vm-0.leeolayvar.koding.kd.io`.
  
  You will have to enter your local SSH password, if you chose one when you
  created your key. After that, presented with `username@vm-X:~$`, singaling
  that you have connected successfully.
  
  This step has quite a few Gotchas so please review them below.
  
  Two likely gotchas, are
  [Possible Gotchas: Agent Failure](#agentfailure) and
  [Possible Gotchas: ssh_exchange_identification](#openbrowser) below.
  
  ### Error: Could not chdir
  
  This "error" can be ignored. I'm not sure what exactly causes it, but
  everyone experiences it and it does not cause any problems.
  [@kiwigeraint](https://koding.com/kiwigeraint) says to simply ignore it. :)



<a name="confirminginstall"></a>
## Confirming your Installation

The best way to confirm your installation is simply by connecting to your
Koding VM. For information on this, see Step #3 above.




<a name="gotchas"></a>
## Possible Gotchas


<a name="agentfailure" class="anchor"></a>
### Agent admitted failure?

If you receive an error when you attempt to connect, matching the following
description:

```
Agent admitted failure to sign using the key.
```

Then run the command `ssh-add`. For additional information on this gotcha,
see the
[Github Help Page](https://help.github.com/articles/error-agent-admitted-failure-to-sign)
on the subject.



<a name="openbrowser" class="anchor"></a>
### ssh_exchange_identification / Keep Koding Open in Browser

Until Always-On VMs are available, Koding will shut down your VM after
approximately ~20 minutes. To ensure that you are able to connect properly
make sure you have your Broser open **and logged in** to Koding.com.

If you experience the following error:

```
ssh_exchange_identification: Connection closed by remote host
```

Your VM being "off" is the most likely culprit. You can ensure it is on by
visiting it's direct url, such as http://vm-0.username.kd.io, and ensuring
you are presented with your Hello World Apache response. If you receive
an error indicating that VM does not exist or is not on, you should have a
good idea why SSH is failing to connect.

If your VM is toggled off, it can be turned on by going to
[koding/Develop](https://koding.com/Develop) and looking for your VM under
**Your Environments**, in the lower left. An image is below for reference:
![Make sure VM is ON](onvm.png)



<a name="authorizedkeys" class="anchor"></a>
### ~/.ssh/authorized_keys

You do *not* need to use authorized_keys in the Koding VMs. The
Account Settings -> SSH Keys takes care of the authorization for you.





<a name="additionalresources"></a>
## Additional Resources

- [Agent Failure Information](https://help.github.com/articles/error-agent-admitted-failure-to-sign)


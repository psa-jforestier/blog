Let's say you have an AWS EC2 instance running Linux.
Usually, this instance is available from a network SSH connection, with a private key and eventually an account and password.
But for a reason, you do not have the private key anymore, or your lost the password.
Is there a way to reset the user password and to get again access to the instance ?

Yes there is.

We will use :
- AWS EC2 Serial console to have a virtual terminal on the instance
- EC2 User data to create a new user, or reset the lost user password.

**AWS EC2 Serial Console**

There are several way for you to connect to a Linux EC2 instances. 

The traditionnal **SSH connection** can be used, as long as you have access to the SSH port (usually :22) of the instance. 

If the port is blocked by a Security Group, you can also use the **Session Manager** service from AWS, which emulate a SSH connection (without the private key) in your browser. Security is assumed by your AWS role.

An other option, is to use the **Serial Console**, which emulate a tty connection on a serial link. The advantage of this type of connection, is that it does not rely on the network stack of the instance. 
It can work even if the machine does not have access to the network. It act like if you are connected with a good old serial cable in front of the server. 
So with this connection type, you are almost guarantee to have a login prompt at any time.

But what if you do not have the user password ?

You have to change the user password to a known value.

**EC2 User Data**

This data is attribute of the instance. You can view this data by selecting the instance and go to Actions > Instance settings > Edit user data. To modify the user data, you have to stop the instance (or "force stop" if you are not able to stop it gracefully).
When the instance is stopped, you can edit the user data, and copy/paste the following script :
```
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"

#!/bin/bash
sudo useradd -m newuser
echo "newuser:123456" | sudo chpasswd
sudo newuser -aG sudo newuser
--//
```
Then restart the instance.
The script is based on the "cloud-init" principle, which is supported by almost any Linux images provided by AWS. The enclosed script will be run **each time the instance is restarted**. 
So each time the machine is rebooted, it will create a new user called "newuser", with password "123456". If the user alread exists, the password will be reset to this value. And also, this user will have a "sudo" capability.
Do not try to reset password for cloud users like "ubuntu" or "ec2-user" because it may not work depening of the OS. Use a brand new created user name.

So by chaining this two hints (Serial console and password reset) you will be able to recover on any EC2 instances as long as you have access to this two features in the AWS web console. 
For example, if someone create instances to mine bitcoin with an unkown SSH key, you are able to log into the instance and stop the process. Be aware that the instane must be rebooted, so IP address may change.

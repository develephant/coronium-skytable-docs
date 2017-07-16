__Coronium SkyTable__ is eligible for the EC2 free tier using Amazon Web Services.

!!! note
    This guide assumes that you have an active AWS account, and are familiar with managing EC2 instances.

### Create A New Instance

Once you log into the __[AWS Console](https://aws.amazon.com/console/)__, navigate to the __EC2__ service. Click the __Launch Instance__ button.

On the next screen, find the __Ubuntu Server 16.04 LTS (HVM)__ AMI, and click the __Select__ button on the right.

Select your preferred instance type. A __t2.micro__ is a good starting point. You can always increase the size later.

Click the __Add Rule__ button on the __Configure Security Group__ screen, and add the following:

|Type|Protocol|Port Range|Source|
|----|--------|----------|------|
|Custom TCP|TCP|7173|Anywhere|

Adjust any additional settings, and then __Launch__ the AMI.

### Connect To The Instance

Once your instance is in a __running__ state, note the __IPv4 Public IP__ address.

Using a terminal/shell of your choice, SSH into the instance.

!!! tip
    You can use the built in shells on both OSX and Linux. For Windows, check out __[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)__.

```
ssh -i <path/to/.pem> ubuntu@<your-instance-ip>
```

_Replace __<your-instance-ip\>__ with the address that was assigned to your instance._

Once you are connected to the droplet, copy and paste the following line into the terminal:

`curl -LO https://s3.amazonaws.com/coronium-skytable/ami.sh && sudo bash ./ami.sh`

Once the installation is complete, __Coronium SkyTable__ is ready for action. Log out of the server by typing __exit__ on the command line.

If you need to edit the config, view logs, etc. then log in with the __coronium__ user.

```
ssh coronium@<your-instance-ip>
```

Log files can be found in the __logs__ directory.

Config options can be changed in __lib/coronium/config.lua__

!!! note
    The default password for a fresh install is: __coroniumadmin__.

!!! warning
    You should change the default password after the install. Making sure to log in as the __coronium__ user, enter __sudo passwd coronium__ in the shell, and then follow the prompts.

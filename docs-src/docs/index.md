![logo](imgs/logo256.png)

# Coronium SkyTable

__A performant user scoped data table store and client for use with [Corona](https://coronalabs.com).__

## Installation

__Coronium SkyTable__ runs best on a [DigitalOcean](https://m.do.co/c/cddeeddbbdb8) __Ubuntu 16.04__ droplet.

!!! tip
    If you're new to [DigitalOcean](https://m.do.co/c/cddeeddbbdb8) please consider signing up with __[this link](https://m.do.co/c/cddeeddbbdb8)__. Not only will you receive a $10 credit (2 free months), but it also helps support the continued development, and testing of __Coronium SkyTable__.


### Create A New Droplet

Once you log into your [DigitalOcean](https://m.do.co/c/cddeeddbbdb8) account, click the "Create Droplet" button.

![step1](imgs/step01.png)

On the next screen, first select a __Ubuntu 17.04__ droplet distribution.

![step2](imgs/step02.png) 

Select your preferred droplet size. A __512MB/1 CPU__ droplet is a good starting point. You can always increase the size later.

![step3](imgs/step03.png)

Next, select a region for the droplet. Consider choosing a location closest to your most active user base.

![step4](imgs/step04.png)

Select your SSH profile to attach to the droplet.

![step5](imgs/step05.png)

!!! caution
    There is an option where you can use a password instead of an SSH key, which may be easier if you're only testing __Coronium SkyTable__, though I wouldn't recommend it. You can learn more about generating SSH keys for [DigitalOcean](https://m.do.co/c/cddeeddbbdb8) here: [Creating SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets).

Now give your droplet a hostname.

![step6](imgs/step06.png)

_Be sure to replace `skytable.develephant.com` with your own hostname._

And finally, click the "Create" button to spin up the droplet.

![step7](imgs/step07.png)

### Connect To The Droplet

Once your droplet is done spinning up, note the ip address on the overview page.

![step8](imgs/step08.png)

Using a terminal/shell of your choice, SSH into the droplet.

!!! tip
    You can use the built in shells on both OSX and Linux. For Windows, check out __[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)__.

`ssh root@<your-instance-ip>`

_Replace `<your-instance-ip>` with the address that was assigned to your droplet._

Once you are connected to the droplet, copy and paste the following line into the terminal:

`curl -LO https://s3.amazonaws.com/coronium-skytable/install.sh && sudo bash ./install.sh && rm install.sh`

Once the installation is complete, __Coronium SkyTable__ is ready for connections. Log out of the server by typing `exit` on the command line.


If you need to view logs, etc. then log in with the `skytable` user.

`ssh skytable@<your-instance-ip>`


!!! note
    The default password for a fresh install is: __coroniumadmin__.

!!! important
    You should change the default password after the install. Making sure to log in as the `skytable` user, enter `sudo passwd` in the shell, and then follow the prompts.


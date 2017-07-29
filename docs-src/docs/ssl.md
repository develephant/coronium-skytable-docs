# Overview

Secure your __SkyTable__ server by utilizing the free SSL certificate service __[Let's Encrypt](https://letsencrypt.org/)__.

!!! warning
    Setting up a secure server is not a trivial process, so be sure to read through _all_ of the steps before starting, and then follow them carefully.

To be issued a secure certificate, you must have a fully qualified domain name, and the proper DNS set up to serve the domain.

A fully qualified domain name is basically a registered domain name. Where you decide to purchase a domain is up to you. __[GoDaddy](https://www.godaddy.com/)__ is a popular choice. 

Once you have your domain name, you will need to "point" it to your __SkyTable__ server. Most domain registars provide a means of setting up DNS.

You will want to set up a 3rd level domain for your SkyTable server. This looks something like:

__skytable.<mydomain\>.com__

### Amazon

When you first install SkyTable, make sure to add port 443 to your security group settings. You can then use the [Route 53](https://console.aws.amazon.com/route53/home) service for your DNS. There are ample guides to assist you. 

Once set up, proceed to the [Let's Encrypt](#lets-encrypt) section below.

### DigitalOcean

You will need to point your domain to the DigitalOcean nameservers. The process for this varies by domain registar, but in all cases, you will need the DigitalOcean nameserver addresses, which are:

  - ns1.digitalocean.com
  - ns2.digitalocean.com
  - ns3.digitalocean.com

_Instructions for setting nameservers on GoDaddy can be found [here](https://www.godaddy.com/help/set-custom-nameservers-for-domains-registered-with-godaddy-12317)._

!!! important
    __Spin up a SkyTable droplet as outlined in the [DigitalOcean Installation](install/digitalocean) section before continuing.__

1\. In the DigitalOcean control panel, click the __Networking__ link at the top:

![ssl-step1](imgs/ssl-step01.png)

2\. Enter your new domain name, without any prefix:

![ssl-step2](imgs/ssl-step02.png)

3\. Click the __Add Domain__ button:

![ssl-step3](imgs/ssl-step03.png)

4\. On the next screen, do the following:

 - Enter the hostname (only the domain prefix) of your SkyTable server. (1)
 - From the __WILL DIRECT TO__ field, select your SkyTable droplet. (2)

![ssl-step4](imgs/ssl-step04.png)

5\. Click the __Create Record__ button on the right:

![ssl-step5](imgs/ssl-step05.png)

At this point your DNS is set up, but generally needs to propagate. This can take anywhere from 5 minutes to a number of hours (though usually within 15 minutes). 

You can check the progress using a site like [whatsmydns](https://www.whatsmydns.net/#A/). Enter the full domain, including the prefix, to test.

## Let's Encrypt

!!! Danger
    ___Do not continue with this guide until you have an active domain name for your SkyTable server that you can reach through your web browser.___

To move your SkyTable server over to HTTPS, perform the following steps:

1\. Log into your SkyTable droplet using the __root__ user:

```sh
ssh root@<your-skytable-domain>
```

_Note: The root user is __ubuntu__ if hosting on Amazon._

If you have not changed the password yet, the default is __coroniumadmin__. You may be prompted for your password at various times during this process.

2\. Copy and paste the following on the command line to download the SkyTable SSL updater:

!!! warning
    At this point make sure you're ready to move over to HTTPS. The following process will permanently modify your configuration settings.

`curl -LO https://s3.amazonaws.com/coronium-skytable/ssl.sh && sudo bash ./ssl.sh`

3\. Generate the certificates from Let's Encrypt.

It's a good idea to install a staging certificate first, to make sure the HTTPS endpoint is working properly. Let's Encrypt limits you to 5 updates to the _production_ certificate per day. So use a staging certificate for testing.

To install a staging certificate, on the command line, run the following:

```sh
sudo certbot certonly --standalone --staging
```

You will be prompted for your domain name to issue the certificate against. __Use the full domain name (skytable.<your-domain\>.com)__. You will also need to supply a valid email address.

4\. When the process is complete, you will need to restart __nginx__:

```sh
sudo monit restart nginx
```

You can now vist your SkyTable server at __https://skytable.<your-domain\>.com:7173__.

!!! note
    A staging certificate will generally be blocked by your web browser. This is normal, and confirms that the HTTPS endpoint is active. You can usually override the browser block, and see the server respond for further confirmation.

5\. When you're ready to install your production certificates, run the following:

```sh
sudo certbot certonly --standalone
```

If __certbot__ complains of an existing certificate, select the re-issue option.
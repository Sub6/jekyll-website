---
layout: post
title: Install Yunohost on linode.com
meta_description: How to install Yunohost on a Linode.com VM
author: malte_sanders
date: '2021-05-11 08:32:00'
categories: cloud server selfhosting Linux misc
---

Since they are very simple to use and kind of cheap I thought I might give [linode.com](https://www.linode.com) a shot for selfhosting. As a platform I decided on [Yunohost](https://yunohost.org/), because it is versatile and very easy to setup.

Grab a coffee or tea and let's go. The process takes around 10-15 Minutes.

--------------
## Linode configuration
Go to [linode.com](https://www.linode.com) and setup an account. Look for some promotional codes. I got 100 Dollar to be used in three month for free. 

After you signed up and logged in, click on ***Create*** and choose Linode.

![Create Linode](/assets/img/uploads/yunohost/create_linode.png)

For the setup choose something like this (alter if you feel like it and know what you are doing). Parameters not described can be left blank.

| Parameter | Value
|-|:-|
| `Image` | Debian 10 
| `Region` | choose whatever you like or is close
| `Linode Plan` | Nanode 1 GB or Linode 2 GB or bigger
| `Linode Label` | any name you thing fits your project
| `Root Password` | Password for the VMs root

<br>

After you have verified your selection click on ***Create Linode*** on the right side. Wait while the VM is beeing created.<br>

You can see the progress on the left corner.
![Linode Setup](/assets/img/uploads/yunohost/linode_setup.png)

When the creation is finished and the VM is booted you will see a green dot and ***Running***.

Click on Launch ***LISH Console***. Another window will open and you will be asked for the login (root) and the password (root password you chose during setup)

The VM Image is farely up-to-date, but just to be sure.

{% highlight shell %}
apt update
apt upgrade
{% endhighlight %}

Now lets do the Yunohost install.

{% highlight shell %}
curl https://install.yunohost.org | bash
{% endhighlight %}

Some prompts will ask you about configuration details. Read and agree. Now wait and drink your coffee or tea.

After the setup finished head over to the shown adress, or look in in you Linode Browser Tab for the ***Network*** configuration for the reverse DNS Adress.

Head over to one of those. Your browser will tell you that the connection is not secure. Ignore this for the moment and continue with the setup as the wizard will guide you through the steps. For more informations head over to the [Admindoc of Yunohost](https://yunohost.org/de/admindoc)

Now what is left for you to do if you like:

1. Setup and connect your Domains [More Informations](https://yunohost.org/de/dns_config)
2. Generate your Lets Encrypt Certificates. [More Informations](https://yunohost.org/de/certificate)
3. Install all the applications you want, setup Users and play around

Have fun with your own kind of selfhosted cloud.
---
layout: post
title: Customizing Airplane Mode in Android
meta_description: A way to actually fine tune what is turned off if Airplane Mode is activated 
author: tung_thai
date: '2021-04-12 13:32:00'
categories: android airplane_mode misc
---

You want to turn on **Airplane Mode** on your Android phone, but don't want Bluetooth to be disabled every time? Or even, you want to customize what to turn off with Airplane Mode?

Very well, I can give you a rough idea of how that can be done. You can do this for every Android Version with at least an API Level 17. That means, this is possible since Android 4.2.

All you need is a pc or laptop, which can run **adb** (Android Debug Bridge). That's it. No third party app or software, only tools which you can fetch from Google directly.

> Android Debug Bridge (adb) is a versatile command-line tool that lets you communicate with a device. The adb command facilitates a variety of device actions, such as installing and debugging apps, and it provides access to a Unix shell that you can use to run a variety of commands on a device.
> <cite>from Google [1]</cite>

[1]: https://developer.android.com/studio/command-line/adb

It will be used to access and set some settings directly on your phone. But only for that? Tsk, do not underestimate adb, since it can do many more things than that! But for the sake of this article, we settle down for the customization of our Airplane Mode.

## Install adb
--------------

### Linux

If you have Linux, go into your repository and just install the package **adb**. In Debian, Ubuntu or PopOS! for example, all you need to do is running following command in terminal:

{% highlight bash %}
% sudo apt install adb
{% endhighlight %}

Done.

If, for whatever reason, you want to install that package directly from Google, open [this](https://dl.google.com/android/repository/platform-tools-latest-linux.zip). It is a link directly to the latest ZIP file for download. Follow the instruction in that package.

### Windows / Mac

Check if you have the right USB driver at all. Please visit Googles help for this [here](https://developer.android.com/studio/run/oem-usb.html){:target="_blank"}.

Then you need to download and install adb for [Windows](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) or [Mac](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip). These links will lead to the latest corresponding ZIP packages. Follow the instruction in that package.


## Prepare adb and your phone
-----------------------------

After the installation is done, we need to prepare your phone first.

On phone, access the **Developer Options** and enable **USB Debugging**. If you don't know what I'm talking about, start from step 1, else go on with step 2.

1. Go into **Settings**, find the Build Number and tap on it seven times. Now  Developer mode will be enabled.
2. Go back into Settings, tap on Developer Options, find the option USB-Debugging and enable it. You will be asked something like _Allow USB debugging?_ Agree to that by tapping on _OK_. By the way, you will be asked this every time if you enable that again.

Now connect you phone to your pc with adb via physical USB cable. Call up the command line or terminal on your pc and type:

{% highlight bash %}
% adb devices
{% endhighlight %}

This first command is merely for checking if everything works as desired. If you do this the first time, your phone will ask you to trust the pc it is connected to while displaying an RSA fingerprint of your pc. Also allow it by tapping _OK_.

Optionally, you can also check _Always allow from this computer_. Then it won't ask you again the next time. After you've confirmed that your phone is ready, running the command mentioned above will yield something like this:

{% highlight bash %}
List of devices attached
<devices series number>	device
{% endhighlight %}

It should display the series number of your phone only. Unplug any other devices, if you have multiple devices listed here to keep everything sane and simple.

Now we're finally ready to explore the Airplane mode setting.


## Customize Airplane Mode on your phone
----------------------------------------

Let's start by entering the shell of your Android device with the command `adb shell`. If successful, it will look like this:

{% highlight bash %}
% adb shell
brot:/ $ _
{% endhighlight %}

Typically instead of `brot`, there will be an abbreviation of your device name. From here, you might try to type:

{% highlight bash %}
$ settings get global airplane_mode_radios
{% endhighlight %}

### Explanation

The command `settings get global` is used here to get (read only) the current status of `airplane_mode_radios`, which is actually the list of every available radios which will be disabled if Airplane Mode is enabled. The available radios are:

| Keyword | Explanation
|-|:-|
| `cell` | Cellular radio, which includes GPRS (2G), UMTS (3G), LTE (4G) and 5G.
| `wifi` | Wireless Fidelity, a radio network following the IEEE-802.11-standard. WiFi is just a subset of WLAN (a radio network without further specifications). Actually, there are also other types of radio standards for local radio network too, but since WiFi is practically used everywhere as the WLAN technology, the incorrect assumption of both of them being the same... happened.
| `bluetooth` | Short-range wireless technology standard. It is mostly used for wireless headset, gaming pads or fitness tracking devices in our everyday life cases.
| `nfc` | Near Field Communication. Communication protocol between devices over a distance of only 4 cm or less. Works with induction and is relatively slow. Usually used for payment systems or often integrated in identity cards of all sorts for identification, both without contact.
| `wimax` | Worldwide Interoperability for Microwave Access, a family of wireless broadband communication standard based on the IEEE-Standard 802.16. It is sometimes also counted as one of the 4G technologies.

To be honest, I didn't know about WiMAX at all until I started to look it up. If you're not living in the USA, chances are high that you don't know about this technology, me neither.

### For Example

Let's say, you have some active bluetooth connections running, be it because of a smartwatch, your headset or a gaming pad. Sometimes you want to turn on Airplane Mode to cut all internet connections to prevent your contacts to constantly disturbing you. But you still want to listen to the music currently running or just prevent your smartwatch to lose connectivity every time. Even if it's only until you turned on Bluetooth manually again, it can be quite annoying, having to do that Every. Single. Freaking. Time.

The following setting will help you with that:

{% highlight bash %}
$ settings set global airplane_mode_radios cell,wifi,nfc,wimax
{% endhighlight %}

With this, every time you switch on Airplane Mode, all radios will be turned off except for `bluetooth`, since we left out that keyword in the settings. In fact, Airplane Mode won't touch your Bluetooth setting anymore. Every keyword you left out in above setting, it will be ignored if it is activated.

Just remember to reboot your phone for the settings to actually take effect.

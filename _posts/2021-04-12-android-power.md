---
layout: post
title: Customized fly mode in Android
meta_description: A way to actually fine tune what are turned off if fly mode is activated 
author: JohnDeep
date: '2021-04-12 13:32:00'
categories: android fly_mode misc
---
You want to turn on `Airplane Mode` on your Android phone, but don't want Bluetooth to be disable every time? Or even, you want to customize what to turn off with `Airplane Mode`?

Very well, I can give you a rough idea of how it's done. You can do this for every Android Version with at least an API Level 17. That means, this is possible since Android 4.2.

All you need is a pc or laptop, which can run `adb` (Android Debug Bridge). No third party app or software, only tools which you can get from first party directly.

> Android Debug Bridge (adb) is a versatile command-line tool that lets you communicate with a device. The adb command facilitates a variety of device actions, such as installing and debugging apps, and it provides access to a Unix shell that you can use to run a variety of commands on a device.
> -- <cite>from Google [1]</cite>

[1]: https://developer.android.com/studio/command-line/adb

It will be used to access and set some settings directly on your phone. Only for that? Do not underestimate `adb`, since it can do many more things than only for this! But for the sake of this article, we settle down for the customization of our `Airplane Mode`.

## Install `adb`
---

### Linux

If you have Linux, then go into your repository and just install the package `adb`. In Debian, Ubuntu or PopOS! for example, it would work like this:

```
% sudo apt install adb
```

Done.

If, by any chance, you want to install that package directly from Google, open [this](https://dl.google.com/android/repository/platform-tools-latest-linux.zip). It is a link directly to the latest ZIP file for download. Follow the instruction in that package.

### Windows / Mac

First check if you have the right USB driver at all. Please visit Googles own help [here](https://developer.android.com/studio/run/oem-usb.html).

Then you need to download and install `adb` for [Windows](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) or [Mac](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip). These links will lead to the latest corresponding ZIP packages. Follow the instruction in that package.


## Prepare `adb` and your phone
---

First of all, you need to access the `Developer Options` and enable `USB Debugging`. If you don't know what I'm talking about, start from step 1, else proceed with step 2.

1. Go into `Settings`, find the Build Number and tap on it seven times. Now  Developer mode will be enabled.
2. Go back into `Settings` and then tap on `Developer Options`, find the option `USB-Debugging` and enable it. You will be asked something like _Allow USB debugging?_ Agree to that by tapping on _OK_. By the way, you will be asked this every time if you enable that again.

Now connect you phone to your pc with `adb` via physical USB cable. Call up the command line or terminal on your pc and type:

```
% adb devices
```

This first command is merely for checking if everything works as desired. If you do this the first time, your phone will ask you to trust the pc it is connected to while displaying an RSA fingerprint of your pc. Also allow it by tapping _OK_.

Optionally, you can also check _Always allow from this computer_. Then it won't ask you again the next time. After you've confirmed that your phone is ready, run the command mentioned above will yield something like this:

```
List of devices attached
<devices series number>	device
```

It should display the series number of your phone only. Unplug any other devices, if you have multiple devices listed here to keep everything sane and simple.

Now we're finally ready to explore the `Airplane mode` setting.


## Customize `Airplane Mode` on your phone
---

Let's start by entering the shell of your Android device. If successful, it will look like this:

```
% adb shell
brot:/ $ _
```

Typically instead of `brot`, there will be an abbreviation of your device name. From here, you might try to type:

```
$ settings get global airplane_mode_radios
```

### Explanation

The command `settings get global` is used here to get (not change) the current status of `ariplane_mode_radios`, which is actually the list of every available radios which will be disabled if `Airplane Mode` is enabled. The available radios are:

| Keyword | Explanation |
| ------- | ----------- |
| `cell` | Cellular radio, which includes GPRS (2G), UMTS (3G), LTE (4G) and 5G. |
| `wifi` | Wireless Fidelity, a radio network following the IEEE-802.11-standard. WiFi is just a subset of WLAN (a radio network without further specifications). Actually, there are also other types of radio standards for local radio network too, but since WiFi is practically used everywhere as the WLAN technology, the incorrect assumption of both of them being the same... happened.
| `bluetooth` | Short-range wireless technology standard. It is mostly used for wireless headset, gaming pads or fitness tracking devices in our everyday life cases.
| `nfc` | Near Field Communication. Communication protocol between devices over a distance of only 4 cm or less. Works with induction and is relatively slow. Usually used for payment systems or often integrated in identity cards of all sorts for identification, both without contact.
| `wimax` | Worldwide Interoperability for Microwave Access, a family of wireless broadband communication standard based on the IEEE-Standard 802.16. It is sometimes also counted as one of the 4G technologies.

To be honest, I didn't know about WiMAX at all until I started to looked it up. If you're not living in the USA, chances are high that you don't know about this technology, me neither.

### Set some Examples

Let's say, you have some active bluetooth connections running, be it a smartwatch, your headset or a gaming pad. Sometimes you want to turn on `Airplane Mode` to cut all internet connections to prevent your contacts to constantly disturbing you. But you still want to listen to the music currently running or just prevent your smartwatch to lose connectivity every time. Even if it's only until you turned on Bluetooth manually again, it can be quite annoying, having to do that Every. Single. Time.

The following setting will help you with that:

```
$ settings set global airplane_mode_radios cell,wifi,nfc,wimax
```

Every time you switch on `Airplane Mode`, all radios will be turned off except for `bluetooth`, since we left out that keyword in the settings. In fact, `Airplane Mode` won't touch your Bluetooth setting anymore. Every keyword you left out in above setting, it will be ignored if `Airplane Mode` is activated.

Just remember to reboot your phone for the settings to actually take effect.

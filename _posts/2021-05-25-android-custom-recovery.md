---
layout: post
title: Custom ROM for an old (Samsung) Tablet or Phone with Android
meta_description: You want to let your quite old (Samsung) tablet or phone be reborn for new uses?
author: tung_thai
date: "2021-05-25 15:32:00"
categories: android custom_rom root twrp recovery samsung linux heimdall
---

Do you have a very old tablet or smartphone with its warranty expired long ago already? Is it a device you still want to use no matter what? But you are concerned about security, since it runs a very old Android, something around Lollipop (5.x.x) or even older for example?

Very well, I tell you **one way** how to deal with this issue. It will be about running a custom ROM on your Android device.

## Cons and Pro

---

First the reasons why you should not do this.

- **This process will void the software warranty of your device**. In most cases it is the warranty of the whole device, but in some countries, you might have a law that separates the warranty of any devices by hardware and software.

- If you do not plan ahead, **you will lose all data on the device**. But this can be prevented by saving all the data beforehand, even though the copying can take time, depending on how much data you have in the first place. This is not a real problem, but more of a hassle to do.

- Nowadays, installing a custom ROM has a high success rate, but it will never be 100 percent. **It is thus possible to make the device completely unusable**.

Not the reasons why you should try this.

- If your device is too old for any vendor updates anyway, **you can make it usable again** via custom ROM. You might be able to run many new apps again even. Also, why buying a new device, if you are still happy with your old one?

- Newer devices are often preloaded with so-called bloatware, software which cannot uninstalled. Custom ROMs are usually preloaded with only the utmost important software. **It is in general free from bloatware** in contrast to a stock vendor device.

- Since preloaded Google software is not allowed, **all custom ROMs are free of Google software**. But some custom ROMs go much further and completely shut down every telemetry to Google even.

- And at last, **running an old Android version is a security risk in itself**. Old Android versions do not receive any updates anymore, neither functionality nor security updates.

## General Process Overview

---

The general steps to get a custom ROM onto your device of choice is quite straight forward:

- Find an appropriate custom ROM of your liking to install
- Prepare _heimdall_ (or _fastboot_)
- Unlock the device's bootloader if needed
- Install a custom recovery image
- via custom recovery, install custom ROM

To be able to execute the steps above, you need to physically connect your device to a PC via USB cable. I am using Linux for this process, so some minor details might be different in Mac or Windows, but the general steps should be the same.

## Appropriate Custom ROM, Gapps and Recovery

---

There are no best custom ROM out there, but if you are new to this, it is best to start out with something generally known. **LineageOS** is one of the best known among custom ROMs. It is said to be one of the most stable custom ROMs available and has a wide range of supported devices. It is thus ideal for beginners.

Of course, it is only a suggestion. You are free to use any other custom ROMs available for your device. To gather more information on this, you should visit the [XDA Developer Forums](https://forum.xda-developers.com/). It is the best known place to find more information about your device and what custom ROM is actually available for that specific device.

Even though the website changes sometimes, in general, many devices have their own forum for discussion. It is in most cases _the_ gathering spot also for developers themselves, where they can announce and share their custom ROMs to the world.

For example, my device is a _Samsung Tablet 4 10.1_, the XDA website luckily has a forum section generally called _Samsung Galaxy Tab 4_, which also includes my device.

All custom ROMs do **not** have the Google packages preinstalled, which is actually good, since there are people who want to avoid Google anyway. But it can be easily installed afterwards via so called [**gapps**](https://opengapps.org/). For the Galaxy Tab in my case, I needed to choose **ARM** as Platform (_not_ ARM64!). You need to choose the correct Android version to match your custom ROM or else the installation later on will fail. By the way, I am recommending the **pico** variant, since it provides only the most necessary packages for Google Play to work. Everything else can be installed afterwards if needed. Also, LineageOS seems to be much more stable with the pico variant of gapps in my case.

Usually, you will be able to find various custom ROMs there and also the appropriate **recovery**, since they sometimes assume that you do not have it yet.

The recovery image we want to use is **TeamWin - TWRP** or just **TWRP**. In my case, I needed to get TWRP from [here](https://forum.xda-developers.com/t/recovery-3-5-2_9-unified-matisse-millet-twrp-recovery.4019831/), since on the official site of TWRP, there is no build anymore.

In case for your device, try to look through the forum of your device for TWRP while you are at it. You also might try to search at [Android File Host](https://www.androidfilehost.com/), since many developer host their files on that platform. You might have luck to find a very recent package of TWRP and a custom ROM of your choice.

I assume that you have never done this before, so you need to follow the instructions over there carefully to get the correct image file for further progress.

## Prepare Fastboot or Heimdall for Recovery

---

Depending on your device, you need one of the tools to get the custom recovery onto your device. To get to the point, you only need **heimdall**, if your device is from Samsung. Every other vendors would require **fastboot** instead. The reason is that Samsung devices do not support fastboot for some reasons. Thus, exclusively for Samsung, heimdall was created. In this article, I am working with heimdall, since I only have a Samsung device to work with.

What is the _recovery_ by the way? As the name suggests, it is literally the part of your device for recovery, backup and some little more. It allows you to reset your device, if you cannot even boot into Android anymore for example. You can also update your device manually if needed by putting another stock ROM on it. But unfortunately, it cannot be used to flash a custom ROM, since manufactures usually do not allow unauthorized ROMs on their devices.

### Linux

On Debian, Ubuntu or Pop!\_OS for example, all you need to do is running following command in terminal:

{% highlight bash %}
% sudo apt install heimdall-flash
{% endhighlight %}

Done.

For more information, like if you want to install heimdall manually instead, go to the [heimdall page](https://www.glassechidna.com.au/heimdall/) directly to get it there. Carefully follow the instruction provided there.

To confirm heimdall is working correctly, go to the terminal and type:

{% highlight bash %}
% heimdall -v
{% endhighlight %}

You will get a prompt with various information about how to use the tool.

### Windows / Mac

Check if you have the right USB driver at all. Please visit [Googles help](https://developer.android.com/studio/run/oem-usb.html){:target="\_blank"} for this.

Then similar to the manual installation for Linux, visit the [heimdall page](https://www.glassechidna.com.au/heimdall/) for more information.

## Replace the Recovery with TWRP

---

Now as a last warning, you are about to do a currently irreversible step to trip the so called **knox counter** of your device, which is one indicator to detect the warranty of the device. As mentioned before, this process will void the warranty. So make sure to be aware of that.

By turning off the device and then booting into **Download Mode**, you should see something like this:

{% highlight bash %}
ODIN MODE
PRODUCT NAME: <your device name>
SYSTEM STATUS: Official
[..]
KNOX WARRANTY VOID: 0x0
[..]
{% endhighlight %}

The important thing is the **System status** and **Knox warranty void** line, which will change after the process. Search for the way to get into **Download Mode** for your device, since it varies from vendor to vendor. In my case, I needed to shut down the device first, then turn it on while holding **Volume Down**, **Home Button** and the **Power Button** for some seconds.

After successfully booting into Download Mode, you will get a warning in the beginning about how custom OS can cause critical problems and so on. It is true, if you get some dubious custom ROM on your device, you will have to be careful of possible problems. 

From there, proceed with _Continue_, which is usually done by hitting **Volume Up**.

Now switch to the PC and prepare the TWRP file you have downloaded for your device. In my case, it is a file named `twrp-<version>-matisse.tar`, which you have to unpack. You will find a single file named `recovery.img`.

Next, connect your device via USB cable to your PC, then go into the folder with the unpacked recovery file, open a Terminal window here and type:

{% highlight bash %}
% heimdall devices
{% endhighlight %}

This shows if it recognized your device correctly. Proceed with the following command:

{% highlight bash %}
% heimdall flash --RECOVERY recovery.img --no-reboot
{% endhighlight %}

Hit enter and wait for this process to be **thoroughly** done. If you interrupt this process by unplugging the USB cable, switching off the PC or even just breathing (last one is a joke) for example, you risk bricking your device. So be patience.

You will see when it is done if you can see the terminal shows a prompt again. It is important to have the `no-reboot` flag to prevent immediate reboot of your device. In some cases, if a reboot follows, the custom recovery might be overwritten again with the stock recovery.

From Download Mode, reboot directly into recovery instead to make the change done via heimdall permanent. With this, your device is now able to install every custom ROM available. Furthermore, your device is now half rooted. Only half, because you would still need to install some frontend in Android itself to be able to use root.

## At last, Overwrite Stock ROM with Custom ROM

---

Keep the USB cable plugged in, because from the TWRP recovery, you can easily mount your device as storage device for data transfer. We need this to transfer the custom ROM zip-file onto the device.

Last warning, if still not done yet, this is your last chance to back up your data before all data will be completely wiped from your device for a clean installation.

### Transfer of custom ROM file

The goal is to transfer the custom ROM of your choice onto your device for installation. There are two ways to do that, either via **adb** or your favorite file manager. I did it via classic file manager, since TWRP has a **Mount** button exactly for this purpose. With this, your device acts as a storage device, and you can copy the custom ROM very easily onto your device.

After this is done, go to **Install** in TWRP on your device, navigate to the zip-file, select it and swipe to confirm the flashing. This might take a while, since a custom Android will be installed.

Optionally, you can install the **gapps** directly after the custom ROM installation is done and before a reboot.

After everything is installed, it is recommended to wipe the cache/dalvic and data of your device to prevent old config data interfering with new apps. After this is done, you can finally reboot your device into the freshly installed custom ROM. The first time boot can take a while, but rest assured, after everything is set up, any other boot processes will be faster.
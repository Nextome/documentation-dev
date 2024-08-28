---
title: Introduction
hide:
  - footer
---
# Nextome Proximity SDK Documentation

![Nextome Android SDK Cover](../assets/cover.png)

Our **Proximity Software Development Kit** for Android and iOS allows you to create smart applications that notify you when your smartphone enter inside or exit outside a Beacon area.
Is very simple and easy to integrate and it works even if your application has been closed for a long time.

Explore our documentation to get more info about that. 

## How it works

A Beacon device can be identified by a scanner through some informations such as UUID, Major and Minor like that 
```
UUID = "f7826da6-4fa2-4e98-8024-bc5b71e0893e" 
Major = 1 
Minor = 12345
```
With this information, you can tell the OS that you are interested in being notified when you enter or exit a specific area identified by one or more beacons with a particular UUID, or a specific beacon identified by a particular UUID, Major, and Minor.

The Beacon transmits a signal that allows the scanner to recognized it through the values described above. This signal define a spacial area based on its transmit power. When a smartphone with an active scanner enters this area, the operative system translates this event as a **Region Enter**. Similar behavior occurs when the smartphone leaves this area, so the operative system translates this event as a **Region Exit**.

On both Android and iOS platforms, Nextome Proximity SDK works with a background service. After first application launch, through the SDK, the operative system listens to the Beacons registerd on our Portal.
Even your application is killed, the operative system is costantly listening on incoming/outcoming Bluetooth devices.

## Interaction
In our Portal, you can create an **Interaction** to define the behavior of proximity. In details you can define when the interaction must be fired (distance from beacon and the trigger event type like Region Enter, Region Exit or both) and what the interaction must be provide at your application when it is fired (like a specific text, or an image, or more complex data like a Json string).

## What are the limitations?

Android and iOS operative system, adopts some rescritions on Bluetooth usage in background to preserve battery life. Each OS have different behaviors about that.

!!! warning "Both platform, Region limitation"
    In our SDK, **an area is defined by UUID Beacon attribute only** so, if 2 beacon are installed so closely and their signal area intersects each other, the operative system will fire 1 Enter Region only. If the signal of first catched beacon is lost and the signal of second beacon is still receiving, the operative system will translate this as "ok, you are still in the region so nothing changes for me", this means that no Region Event will be fired.

### Android limitations

Android has not limitation about the number of Beacon that can be recognized at the same time, it can be listen for any UUID. 

The Bluetooth background service has a huge limit instead. Starting from Android 8+, the Android operative system limits background activity to once every 15 minutes (the observed interval in our tests is about 10-30 minutes, handled by OS and is different for each smartphone on the market).

Therefore Bluetooth scanner is active for a few seconds every ~15 minutes. In this interval it searches for nearby Bluetooth devices. 
**So, always keep in mind that timely recognition of beacons is not guaranteed with Android.**

!!! info "Check what you can do to prevent Android kill action"
    If you want to deactivate some os limitation on the smartphone, please check [Don't kill my app](https://dontkillmyapp.com/) website.

An alternative way to avoid this limitation is to use an **Android Foreground Service** which allows you to constantly listen to the beacons (it is called Ranging) but it is a heavy process with an high battery power consumption, and the user is constantly notified that the service is running and can stop at any time. At today, foreground service is not supported yet in our Proximity SDK but it is planned.

The major risk with Android is losing Beacon detection if you are outside Beacon advertising area when the scanning process starts.

### iOS limitations

Differently from Android, **iOS operative system support only 20 regions at same time for scanner process**. So when you register your Beacons for proximity, take care of this. Unlike Android, iOS works very well on recognizing beacon regions.

## Android Resources
[Android Docs](Getting%20Started/android-getting-started.md) | [Changelog](Android/changelog.md) <!--| [Example Project](https://github.com/Nextome/nextome-phoenix-android-whitelabel)-->

## iOS Resources
[iOS Docs](Getting%20Started/ios-getting-started.md) | [Changelog](iOS/changelog.md) <!--| [Example Project](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel)-->

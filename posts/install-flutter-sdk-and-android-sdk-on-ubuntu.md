---
date: 2024-03-10
title:  Install Flutter SDK and Android SDK on Ubuntu
tags:
- Flutter
- Android SDK
description: Install Flutter SDK and Android SDK on Ubuntu for develop Android App.
---

# Install Flutter SDK and Android SDK on Ubuntu for develop Android App

## system setup  
* Maybe need tools  
```bash
apt-get install unzip clang cmake git ninja-build pkg-config libgtk-3-dev liblzma-dev libstdc++-12-dev
apt remove android-sdk
apt install openjdk-17-jdk
```
*  Set the `JAVA_HOME` variable in the environment to match the location of Java installation.
```bash
export JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64/"
```

## Install and Setting Android SDK
* Download the latest `command line tools only` package from the `Android Studio` [downloads page](https://developer.android.com/studio?pkg=tools).
```bash
wget -c https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip
```
* unzip the package to the `Android SDK` dir  
```bash
mkdir -p /opt/android-sdk/cmdline-tools
mv commandlinetools-linux-11076708_latest.zip /opt/android-sdk/cmdline-tools/
cd /opt/android-sdk/cmdline-tools
unzip commandlinetools-linux-11076708_latest.zip
mv cmdline-tools latest
# the prefix path is like /opt/android-sdk/cmdline-tools/latest
```
* To install a previous version of the command-line tools, you can use `sdkmanager`,command like: (Optional)
```bash
/opt/android-sdk/cmdline-tools/latest/bin/sdkmanager --install "cmdline-tools;version"
#Substitute version with the version want to install, for example 6.0
```
So,CI server not need install `Android Studio` and just install `command line tools` include `sdkmanager` to manager the other SDK package and setting SDK tools to build app.  
* Install `Android SDK Platform Tools`
use sdkmanager  
```bash
sdkmanager --list
sdkmanager --install platform-tools
```

In addition to downloading from the SDK Manager, you can download the SDK Platform Tools from [here](https://developer.android.google.cn/tools/releases/platform-tools#downloads.html) 
```bash
wget -c https://dl.google.com/android/repository/platform-tools_r35.0.0-linux.zip
```

## Environment Variables Setting
```bash
# Android SDK
export ANDROID_HOME=/opt/android-sdk
export PATH="$PATH::/opt/android-sdk/cmdline-tools/latest/bin:/opt/android-sdk/platforms"
# Flutter SDK
export FLUTTER_HOME=/opt/flutter
#export FLUTTER_HOME=/opt/flutter3.16.5
export PATH="$PATH:/opt/flutter/bin::/opt/flutter3.16.5/bin"
```

## References
[Command-line-tools](https://developer.android.google.cn/tools)  
[sdkmanager](https://developer.android.google.cn/tools/sdkmanager)

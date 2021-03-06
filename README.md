# React-Native-Install

A summary of the **successful** steps to installing React Native on Linux Mint and Mac OSX. It was a fantastic mix of very exciting and massively frustrating. The online open source community has been invaluable to me throughout my coding journey, and this is my attempt to give back. I hope it helps someone out!

#### Core Versions
* React Native 0.49
* Watchman 4.9.0
* Android Studio 2.3.2


Attracted by the apparently effortless installation process on the documentation, I decided to give React Native a shot.
I was initially drawn to create-react-native-app (CRNA) as it seems to be reducing the time to start coding. I couldn't have been more wrong. At first I was able to get an app up and running combined with Expo in less than 30 minutes. After that, nothing worked well. I ran into issues with trying to run my app on an Android emulator and debugging was turning into a nightmare. Nevertheless, I decided to take a step back and use the classic react-cli.

I reproduced this issue on three separate computers: two Mac Desktops, and one Linux Mint laptop. I took screenshots from the issues on Mac, and from here on out any relative paths will be referencing OSX. Please adapt your code accordingly.

Navigate to Android at https://facebook.github.io/react-native/docs/getting-started.html
to install the correct version of the Android Sdk and Emulators (I did **not** use GenyMotion)

* Add the following to your ~/.bash_profile

```
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

The first line above should be customized. *Please make sure you use the correct Android SDK path.* You can find the actual location of the SDK in the Android Studio "Preferences" dialog, under Appearance & Behavior → System Settings → Android SDK.

* Next, save the updated .bash_profile `source .bash_profile`

* Spin up an Android Emulator from Android Studio with ![Emulator Icon](screenshots/react-native-tools-avd.png?raw=true)
icon from the toolbar

Cool, now lets create a project!
```
npm install -g react-native-cli
```
___
```
react-native init MyFirstReactNativeProject
cd MyFirstReactNativeProject
react-native run-android
```
___

This is going super well so far, this is cruisey! According to the documentation this is all you should need to do to get started. For me, this was simply the first level of errors to beat. Game on!

##### The first error might read:

***
![Error 1](screenshots/Error-1-sdk-dir.png?raw=true)
___

##### What went wrong? Lets see...
**SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable.**

It literally doesn't know where the sdk is, so tell it. Good thing we know where it is already since we included it in our .bash_profile.

```
cd MyFirstReactNativeProject/android
touch local.properties
echo 'sdk.dir=/Users/Shared/Android/sdk' >> local.properties
```

The _echo_ path above will be your unique path to the Android Sdk. Lets carry on. Try again.

```
react-native run-android
```

put some coffee on, it might take a minute..

#### This next error might occur
***
![Error 2](screenshots/Error-2-adb-not-found.png?raw=true)

It can't find Android Debugging Bridge (adb) is, despite, for me, 
being available globally as tested with  `abd devices`

It should output something like this:

List of devices attached
emulator-5554  device

If this is offline, or nothing shows up, check to make sure the emulator is still running.
For me it was resolved by sifting through this issue:
https://github.com/facebook/react-native/issues/11413
##### For Unix
```
brew cask install android-platform-tools
```

##### For Linux, in your project directory:
```
sudo apt-get update
sudo apt-get install adb
```

Awesome once that is done, run it again
```react-native run-android```

Should work now!

#### Extra linux issues

Installing watchman

The blow link should work for you except checkout to **4.9.0** as that is the most stable release
https://medium.com/@vonchristian/how-to-setup-watchman-on-ubuntu-16-04-53196cc0227c

#### If you get an error about:
___
ERROR  A non-recoverable condition has triggered.  Watchman needs your help!
The triggering condition was at timestamp=1444001279: inotify-add-watch(/home/oren/projects/react-native/HackerNews-React-Native/node_modules/react-native/node_modules/yeoman-generator/node_modules/download/node_modules/caw/node_modules/get-proxy/node_modules/rc/node_modules/.bin) -> The user limit on the total number of inotify watches was reached; increase the fs.inotify.max_user_watches sysctl
All requests will continue to fail with this message until you resolve
the underlying problem.  You will find more information on fixing this at
https://facebook.github.io/watchman/docs/troubleshooting.html#poison-inotify-add-watch

* You have to increase the amount of files watchman is tracking. It's a bit different for each machine, but this post led me to the solution https://unix.stackexchange.com/questions/13751/kernel-inotify-watch-limit-reached
* Offical documentation: https://facebook.github.io/watchman/docs/install.html


I hope this helps someone out there in the world! Have fun

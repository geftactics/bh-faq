---
title: Building Android Dev App
subtitle:
tags: []
author: henri
layout: doc_ci
---


#### Sections in this article
{:.no_toc}
* TOC
{:toc}

<!-- 2021-04-07 written by Henri Ly -->

# Setup your Android environment

You will need to get Android Studio : 
https://developer.android.com/studio

## Android Studio

You will need to get Android Studio, download and install it : 
https://developer.android.com/studio

Go by default installation or custom it if you know what you are doing.

## Clone ledger-live-mobile repository and install it

We will begin by installing dependencies using brew

```
brew install ruby watchman yarn
```

Then clone ledger-live-mobile, and cd into the folder.

```
git clone git@github.com:LedgerHQ/ledger-live-mobile.git
cd ledger-live-mobile
```

You can now use the yarn command.

```
yarn
```


once it finish, keep the terminal up and go back to Android Studio

## Creating an emulator

Go back to Android Studio and `Open an Existing Project` : the targeted folder should be : `.../ledger-live-mobile/android`

It will take some time to load the project. We will take this time to create an emulator (or you can use your own android device).

Click the AVD Manager icon on the upper right of Android Studio :
<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="120" src="../../../uploads/images/avd_manager_icon.png" ></div>
<!-- --------------------------------- -->

Create the device that you want to have for test, don't go too low on API level it will may not working.

Then start the created device in the AVD manager.



## Run ledger-live-mobile in the emulator

Go back to your terminal, and add the Android sdk path. 
you will need to locate your SDK path, and add it to your env, it should be some where like  : 
`export ANDROID_SDK_ROOT=/Users/<username>/Library/Android/sdk`

If you have some modification you want to test from your live-common you can 

`yalc add @ledger-live/common` (if you have `yalc publish` your live-common)
and rerun 
`yarn install`

then you should be able to :

`yarn android` and it should be running on your created device


---

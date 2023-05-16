---
title: Running iOS and Android Dev Apps
subtitle:
tags: []
author: leo
layout: doc_ci
---


#### Sections in this article
{:.no_toc}
* TOC
{:toc}

<!-- 2021-03-30 also based on 2177138906 in Confluence -->

# Running the iOS build on macOS

***
This page explains how to run a Ledger Live dev app for iOS on macOS.

## Prerequisites

For iOS: [Building iOS build](52_live_install_ios.md).  
For Android: [Building Android build](53_live_install_android.md).  


## Connecting to Nano S / X

In this part, we'll connect a Ledger Nano S to the application, using the ledger-live-common proxy feature.

You would need nvm to install and use node version 12 easily. Installation details are [here](https://github.com/nvm-sh/nvm).

Once installed, open a new terminal and type:

```
nvm install 12 (if not already done)
nvm use 12
```

Then we're going to clone the ledger-live-common repo:

```
git clone https://github.com/LedgerHQ/ledger-live-common
cd ledger-live-common/cli
yarn
```

Now let's launch the ledger-live-common proxy command:

```
node bin/index.js proxy
```

You should see an output such as this:

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="680" src="../../../uploads/images/2177138906/2193555513.png" /></div>
<!-- --------------------------------- -->

- Now, open the LL [DEV] application on the simulator.
- Choose "Ledger Nano X" as the device (even if you're going to use a Nano S).
- "Use initialized device" [...] → Settings.
- On the Ledger logo, click very rapidly until the "Debug" menu appears (a mouse is recommended, as it's virtually impossible with a trackpad.

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="340" src="../../../uploads/images/2177138906/2193227841.png" /></div>
<!-- --------------------------------- -->

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="340" src="../../../uploads/images/2177138906/2193489982.png" /></div>
<!-- --------------------------------- -->

- Go to "Debug http transport" and copy-paste the IP address given
previously by ledger-live-common.

Once that is done, an onboarding page should popup.
- Connect your Nano, and choose the Ledger Nano X device once again.
- Go through the setup.  

Congratulations! You have successfully connected your device

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="340" src="../../../uploads/images/2177138906/2193195145.png" /></div>
<!-- --------------------------------- -->

## Testing on the Nano

- On your ledger-live-mobile directory, double-click on the
ledgerlivemobile.**xcworkspace** file.

It should open XCode. You will have to switch back to the Xcode toolchain (Xcode → Toolchains → Xcode xx.x)

You may want to change the default scheme to either build a debug or a release version (Product → Scheme → Edit Scheme...):

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="720" src="../../../uploads/images/2177138906/2193129601.png" /></div>
<!-- --------------------------------- -->

- Now connect your device to your mac.
- Select it as the target.

You can try to launch a build. If it fails because of a problem with a certificate, then
- Under the ledgerlivemobile menu, go to "Signing & Capabilities"
- Click "Revoke Certificate", XCode will automatically create a new one

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="720" src="../../../uploads/images/2177138906/2193293401.png" /></div>
<!-- --------------------------------- -->

- Try to build it a second time. It should work!

If you need to export the **.ipa** file, click on Product → Archive
After the build is complete you should have this window displayed:

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="720" src="../../../uploads/images/2177138906/2193064065.png" /></div>
<!-- --------------------------------- -->

- Click on Distribute App → Development → Next → Automatically manage
signing,
- Export it to the folder you want to save it in.

You can then do without Xcode or a developer account to install the iOS app by using "Apple Configurator 2".
You download it from the Apple store.

---

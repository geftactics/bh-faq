---
title: Adding *MyCoin* to Ledger Live Desktop and Mobile
subtitle:
tags: []
author: remi_j
layout: doc_ci
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

# Inserting *MyCoin* in Ledger Live Desktop and Mobile

<!--

- [Inserting *MyCoin* in Ledger Live Desktop and Mobile](#inserting-mycoin-in-ledger-live-desktop-and-mobile)
  - [Ledger Live Desktop](#ledger-live-desktop)
    - [Adding the crypto in LLD](#adding-the-crypto-in-lld)
    - [Running the Desktop projet](#running-the-desktop-projet)
  - [Ledger Live  Mobile](#ledger-live--mobile)
    - [Adding the crypto in LLM](#adding-the-crypto-in-llm)
    - [Running the Mobile projet on iOS (Mac only)](#running-the-mobile-projet-on-ios-mac-only)
    - [Running the Mobile projet on Android](#running-the-mobile-projet-on-android)

-->

***

## Ledger Live Desktop

1. Fork https://github.com/LedgerHQ/ledger-live-desktop
2. Clone the repo

The clone will be used to test your modifications locally, using `yalc`.


### Adding the crypto in LLD

1. In  `live-common-set-supported-currencies.js` add the name you gave in the table. E.g., `"mycoin"`
2. Add the error translation keys in `static/i18n/en`.

### Running the Desktop projet

```
git clone <fork-live-desktop>
cd ledger-live-desktop
yalc add @ledgerhq/live-common
yarn install
```

If it is the first time you use `yalc` for live-common do this:

```
cd ledger-live-common
yalc publish
```

## Ledger Live  Mobile

1. Fork https://github.com/LedgerHQ/ledger-live-mobile
2. Clone the repo

The clone will be used to test your modifications locally, using `yalc`.

### Adding the crypto in LLM

1. Similarly step as for instructions for LLD, except it is in file `live-common-setup.js`.
2. Add the error translation keys in  `src/locales/en/common.json`

### Running the Mobile projet on iOS (Mac only)

The prerequisites to running the project are found in the README file.

```
git clone <fork-live-mobile>
cd ledger-live-mobile
yalc add @ledgerhq/live-common
yarn install
yarn run ios
yarn start
```

### Running the Mobile projet on Android

1. Launch the emulatore or plug-in your telephone.
2. Use an Android environment such as Android Studio

```
git clone <fork-live-mobile>
cd ledger-live-mobile
yalc add @ledgerhq/live-common
yarn install
yarn run android
yarn start
```

These pages can be useful

https://ledgerhq.atlassian.net/wiki/spaces/LCH/pages/2177138906/iOS+build+on+macOS

https://ledgerhq.atlassian.net/wiki/spaces/LCH/pages/2548924630/Run+tests+on+emulated+Android+and+iOS

---

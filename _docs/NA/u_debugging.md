---
title: Application Debug
subtitle: Making testing easier
tags: []
toc: true
toc_sticky: true
author: pscott
layout: doc_na
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

## Introduction

<div class="note">

<div class="title">

Note

</div>

Ledger has developed its own emulator called
[Speculos](https://github.com/LedgerHQ/speculos) . Feel free to
checkout:

1.  The [Speculos repo](https://github.com/LedgerHQ/speculos) .
2.  The [Nano S SDK](https://github.com/LedgerHQ/nanos-secure-sdk) and
    the [Nano X SDK](https://github.com/LedgerHQ/nanox-secure-sdk) .
3.  The `speculos` section which gives an overview of how to use
    speculos.

</div>

<div class="warning">

<div class="title">

Warning

</div>

The `BOLOS development environment </u_setup>` is required for
the following article. It applies only for the Nano S, with its SE
firmware either in version 1.5.5 or 1.6.0.

</div>

It is possible to install a debugging firmware on the device's MCU that
will enable printing text outputs from the device to a terminal. To do
so, follow these steps:

1\. First, download the
[updater](https://drive.google.com/open?id=1pbqIDDuamfsvFuEkduCyOFq8mW0HZmeQ)
and the [debug
firmware](https://drive.google.com/open?id=1hTZKqlwKjx51vdqda8SRp_80Yx3lPizb)
and install the python module ledgerblue by running <span
class="title-ref">pip3 install ledgerblue</span>.

2\. Exit any instance of Ledger Live, Ledger Chrome App, or any other
program able to communicate with a Ledger device.

3\. Now, plug your Nano S to your computer while keeping the left button
pressed. Keep it pressed until the screen displays `BOOTLOADER`.

4\. Fire a terminal and move to the directory containing the files
downloaded at step 1.

1.  Install the updater (only if you MCU firmware is not already in
    version 1.11, otherwise just go to step 6):

        python3 -m ledgerblue.loadMCU --targetId 0x01000001 --fileName blup_0.11_misc_m1.hex --nocrc

Wait until `BOOTLOADER` is displayed again on the device's screen.

1.  Install the debug firmware:

        python3 -m ledgerblue.loadMCU --targetId 0x01000001 --fileName mcu_1.11-printf_over_0.11.hex --reverse --nocrc

If you can notice a small `dbg` block at the bottom of the screen, then
it's a success !

<figure>
<img src="../Images/debug_nano.jpg" class="align-center" alt="A Nano S with special debug firmware" /><figcaption aria-hidden="true">A Nano S with special debug firmware</figcaption>
</figure>

Uninstalling this special firmware is also very easy, first you need to
download the [normal
firmware](https://drive.google.com/open?id=1YfdU1dNycojdtuKU_hHctLFzJZzhDFuY),
then you can repeat the installation steps 2 to 5.

Finally, flash the normal firmware with this command:

    python3 -m ledgerblue.loadMCU --targetId 0x01000001 --fileName mcu_1.11_over_0.11.hex --reverse --nocrc

The `dbg` block should now be gone.

### PRINTF macro

The debug firmware enables the `PRINTF` macro, however you have to
define it in your app's Makefile. To do so, add this line in your
Makefile: `DEFINES += HAVE_SPRINTF HAVE_PRINTF PRINTF=screen_printf`

Usually, `PRINTF` is already defined to void with this line
`DEFINES   += PRINTF\(...\)=`. Check if `PRINTF` is already defined
somewhere else in your Makefile, and comment out this definition so it
doesn't override the one that we just set.

<div class="warning">

<div class="title">

Warning

</div>

The PRINTF macro is a debugging feature, and as such it is not intended
for use in production. When compiling an application for release
purpose, please verify that `PRINTF` is disabled in your Makefile. In
other words, in case of release compilation, put back the line
`DEFINES   += PRINTF\(...\)=` and comment out the other one.

</div>

<div class="warning">

<div class="title">

Warning

</div>

The `PRINTF` macro can only be used in between successive calls to
`io_exchange`. Calling it outside of it will result in unexpected
behavior. Behind the scene, `PRINTF` sends a status on the
[SEPH](https://ledger.readthedocs.io/en/latest/bolos/hardware_architecture.html#seproxyhal).
Only one status can be sent in a row, otherwise the SEPH crashes. For
this reason, don't use `PRINTF` just after status-sending calls, such as
`UX_DISPLAY`. This macro packs a call to <span
class="title-ref">io\_seproxyhal\_display</span> and is often the reason
for application crashes. Then is no other work around than to move your
call to `PRINTF` somewhere else in your code.

</div>

This macro can be used in your code like a classical `printf` function
from `stdio.h`. However, it is improved with a neat feature to easily
print byte arrays:

``` c++
uint8_t buffer[4] = {0xDE, 0xAD, 0xBE, 0xEF};

// PRINTF(string, array length, array);
// .*H for uppercase, .*h for lowercase
PRINTF("What a lovely buffer:\n %.*H \n\n", 4, buffer);
PRINTF("I prefer it lower-cased:\n %.*h \n", 4, buffer);
```

<figure>
<img src="../Images/deadbeef.png" class="align-center" alt="Result of the example code printed inside a terminal" /><figcaption aria-hidden="true">Result of the example code printed inside a terminal</figcaption>
</figure>

### Console Printing

The `PRINTF` macro triggers messages from the MCU to the host computer
through the USB link. We use
[USBTool](https://drive.google.com/open?id=16D5vlrbczmBxqpDJml6QUV0RGWs7aZeZ)
to read these messages and print their payload in a terminal.

Unzip the file and execute this command: `./usbtool -v 0x2c97 log`

Now you can launch your app on your Nano S, and every `PRINTF` will end
up printed on the host computer, allowing you to debug your program more
easily.

### PIN bypass

In Ledger app development, it is necessary to enter your PIN code each
time you install an unsigned app. In order to do testing during
development, this means developers wind up using many painful button
presses entering a PIN code compared to relatively few testing their own
application code. The Ledger OS (BOLOS) supports installing a custom
developer certificate. By installing a custom certificate once on your
device you can avoid having to retype your PIN each time you adjust your
app. Here are the steps for the Ledger Nano S:

1.  Generate a public / private keypair using the following command:

        foo@bar:~$ python3 -m ledgerblue.genCAPair
        Public key : 0495331cb86e961fc9cb5792a97737e4204b58be99321dcec463cec3057b3304e9875614004e6e540ab0610a1339fae22df6f6f3ec594912b409d69b72f0eaf390
        Private key: 6c1f1df852255e113b23c2e977d6b5c3ea2aaf411f05a5fdab87a3e8f44468ee

2\. Enter recovery mode on your Ledger Nano S. Do this by unplugging it
then holding down the right button (near the hinge, away from USB port)
while plugging it in again. `recovery mode` should then appear on the
screen. Enter your pin and continue.

3\. Load your public key onto the Ledger Nano S. Paste the public key
generated at step 1 after `--public`. You may need to adjust the
`--targetId` constant to match your device. Find the right targetId for
your device
[here](https://gist.github.com/TamtamHero/b7651ffe6f1e485e3886bf4aba673348).
Notice that we must include a `--name` parameter containing the name of
the custom certificate (any string will do):

    python3 -m ledgerblue.setupCustomCA --targetId 0x31100004 --public yourPublicKey --name dev

If you receive the error `Invalid status 6985` then please review [this
link](https://github.com/LedgerHQ/blue-loader-python/issues/42) and then
go back to step 2. The above command is the simplest that can work
however some developers may wish to use the optional `--rootPrivateKey`
option to specify a secure channel encryption key (which is the private
key generated at step 1) or use the `--name` option for convenient
labeling according to local convention.

4\. Once you have loaded your custom certificate, you can try to load an
app you compiled yourself onto your Ledger to see if you are able to
bypass the PIN. Before you try it, set the `SCP_PRIVKEY` environment
variable to contain the private key generated at step 1 in your shell or
`.bashrc`:

    export SCP_PRIVKEY=yourPrivateKey

and then rebuild/load your app.

For more information see
[loadApp-py](https://ledger.readthedocs.io/projects/blue-loader-python/en/0.1.16/script_reference.html#loadapp-py)

<div class="warning">

<div class="title">

Warning

</div>

A side effect of installing a custom CA on your device is that it will
from now on fail to pass the Ledger Genuine Check, which is required to
install applications from the Ledger Live. To make it genuine again, you
should uninstall your custom CA and all the applications installed
through it.

</div>

Uninstalling a custom CA is very quick:

1\. Enter recovery mode on your Ledger Nano S. Do this by unplugging it
then holding down the right button (near the hinge, away from USB port)
while plugging it in again. `recovery mode` should then appear on the
screen. Enter your pin and continue.

1.  Type this command in your terminal:

        foo@bar:~$ python3 -m ledgerblue.resetCustomCA --targetId 0x31100004

Find the right targetId for your device
[here](https://gist.github.com/TamtamHero/b7651ffe6f1e485e3886bf4aba673348).


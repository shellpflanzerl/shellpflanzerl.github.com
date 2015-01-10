Title: Use a Yubikey to mount encrpyted LUKS volumes
Date: 2015-01-12 08:57
Tags: Linux, Security
Authors: Martin Zehetmayer
Status: draft

A few days ago I got a Yubikey Nano for testing. After a while I found a project where
I could use the help of a small keystorage device like the Yubikey. 

I got a LUKS encrypted partition on my NAS which will not be automatically mounted during
bootup. By now I log in to the system and issue the 'cryptsetup luksOpen' and mount command
manually. This is fine with me but what if other people - like my wife - needs to access
the encrypted data? The password (which exceeds more then 30 letters) is to hard to remember
for someone who does not use it everyday. 

A possible solution is the use of a hardware token which delivers a valid passphrase for the
encrypted device. The Yubikey emulates an USB HID Device and has two configuration slots which
can be programmed to fit different usecases. It comes preconfigured with Yubico OTP on slot 1
(128 bit AES onetime passwords, see https://www.yubico.com/products/services-software/personalization-tools/yubikey-otp/) and a secondary empty slot. 

For the second slot we need the Challenge-Response Configuration. It takes a challenge block of 1-64 bytes and calculates a HMAC-SHA1 on this using the 160-bit secret stored in the Yubikey. The resulting 160-bit hash is sent back as a response which is used as passphrase for the LUKS device. 

The configuration is done with the yubikey personalisation software (GUI version), which can be downloaded from the yubiko website.


To get everything running on my F21 server there are a few steps to take: 

First of all we need the software:

    yum -y install ykpers


Then we can insert the Yubikey in a USB slot and test it: 

    root@nas [0] ~ # ykinfo -a
    serial: XXXXXXXX
    serial_hex: XXXXXXX
    serial_modhex: XXXXXXX
    version: 2.4.2
    touch_level: 1790
    programming_sequence: 3
    slot1_status: 1
    slot2_status: 1
    vendor_id: 1050
    product_id: 10


The Yubikey is now ready. Now we are testing the challenge-response configuration: 

    root@nas [130] ~ # ykchalresp -2 'this is a test'
    bf2e98ba2ee43ab52dc5e32545bae8b11cb06f19

We got a response which will be used as passphrase for LUKS. But we need a better challenge for
this simple text is too easy to guess. Possible methods are the use of /dev/urandom (for example
dd if=/dev/urandom bs=1 count=40 2>/dev/null| base64). 
To mount the volume with this key we need to enter it to a free key slot in the LUKS device.
Normally only slot 0 has a key, but just to be sure we check that: 


    root@nas [0] ~ # cryptsetup luksDump /dev/data/private | grep DISABLED
    Key Slot 1: DISABLED
    Key Slot 2: DISABLED
    Key Slot 3: DISABLED
    Key Slot 4: DISABLED
    Key Slot 5: DISABLED
    Key Slot 6: DISABLED
    Key Slot 7: DISABLED

We add the key:

    cryptsetup luksAddKey --key-slot 1 /dev/data/private
    Enter any existing passphrase:
    key slot 0 unlocked.
    Enter new passphrase for key slot:
    Verify passphrase:
    Command successful.


We put all this in a script: 


#!/bin/bash
challenge="yitwoydCaichewzyagudyanwikjeefJajIrKouQuizdu"
pass=`ykchalresp -2 $challenge`
echo "$pass" | cryptsetup luksOpen /dev/vg_data/private private_data
mount /dev/mapper/myprivate_data /my_private_data


Now we have a fully functional Yubikey configured to mount LUKS devices.
The next step to automate the mount are to call this script when the Yubikey is inserted - 
in Part 2 of this article (coming soon!) 

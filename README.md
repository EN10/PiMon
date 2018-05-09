# PiMon

Monitor mode on the Raspberry pi

* Based on [nexmon](https://github.com/seemoo-lab/nexmon#build-patches-for-bcm43430a1-on-the-rpi3zero-w-or-bcm434355c0-on-the-rpi3-using-raspbian-recommended)

Copy libisl.so.10

    sudo cp libisl.so.10 /usr/lib/arm-linux-gnueabihf/libisl.so.10

**Load nexmon driver**

    ifconfig wlan0 down
    rmmod brcmfmac
    insmod ./nexmon/patches/bcm43430a1/7_45_41_46/nexmon/brcmfmac_4.14.y-nexmon/brcmfmac.ko
    ifconfig wlan0 up
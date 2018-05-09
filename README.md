# PiMon

Monitor mode on the Raspberry pi

* Based on [nexmon](https://github.com/seemoo-lab/nexmon#build-patches-for-bcm43430a1-on-the-rpi3zero-w-or-bcm434355c0-on-the-rpi3-using-raspbian-recommended)

Install Pi Zero W

    sudo su
    apt-get update && apt-get upgrade

    apt install raspberrypi-kernel-headers git libgmp3-dev gawk qpdf bison flex make
    
    sudo reboot
    
    git clone https://github.com/seemoo-lab/nexmon.git
    cd nexmon

    cd buildtools/isl-0.10
    ./configure, make, make install
    ln -s /usr/local/lib/libisl.so /usr/lib/arm-linux-gnueabihf/libisl.so.10
    
Compile Firmware:

    cd /home/pi/nexmon
    source setup_env.sh
    make
    cd patches/bcm43430a1/7_45_41_46/nexmon/
    make
    make backup-firmware
    
nexutils:
    
    cd utilities/nexutil/
    make && make install
    apt-get remove wpasupplicant

Replace Firmware:

    sudo su
    ifconfig wlan0 down
    rmmod brcmfmac
    insmod ./nexmon/patches/bcm43430a1/7_45_41_46/nexmon/brcmfmac_4.14.y-nexmon/brcmfmac.ko
    ifconfig wlan0 up

load the modified driver after reboot:

    cp /home/pi/nexmon/patches/bcm43430a1/7_45_41_46/nexmon/brcmfmac_4.14.y-nexmon/brcmfmac.ko /lib/modules/4.14.34+/kernel/drivers/net/wireless/broadcom/brcm80211/brcmfmac/brcmfmac.ko
    depmod -a
    reboot

** Possible Error **

firmware compiled for wrong kernel?     
Which Kernel:   `uname -a` 4.14?    
Which Firmware: `find -name brcmfmac.ko`    
# PiMon

Monitor mode on the Raspberry pi

* Based on [nexmon](https://github.com/seemoo-lab/nexmon#build-patches-for-bcm43430a1-on-the-rpi3zero-w-or-bcm434355c0-on-the-rpi3-using-raspbian-recommended)

Install Pi Zero W

    sudo su
    apt-get update && apt-get upgrade

    apt install raspberrypi-kernel-headers git libgmp3-dev gawk qpdf bison flex make

    git clone https://github.com/seemoo-lab/nexmon.git
    cd nexmon

    cd buildtools/isl-0.10
    ./configure, make, make install
    ln -s /usr/local/lib/libisl.so /usr/lib/arm-linux-gnueabihf/libisl.so.10
    
    cd /home/pi/nexmon
    source setup_env.sh
    make
    cd patches/bcm43430a1/7_45_41_46/nexmon/
    make
    make backup-firmware
    
    cd utilities/nexutil/
    make && make install
    apt-get remove wpasupplicant

    sudo reboot

    ifconfig wlan0 down
    rmmod brcmfmac
    insmod ./nexmon/patches/bcm43430a1/7_45_41_26/nexmon/brcmfmac_kernel49/brcmfmac.ko
    ifconfig wlan0 up
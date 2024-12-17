SDR++ Installation on RPi 5
Nightly Builds: https://github.com/AlexandreRouma/SDRPlusPlus/releases/tag/nightly
Installing on the Raspberry Pi 5 with Bookworm Debian 64bit
Reference: https://www.aaronrombaut.com/build-sdr-on-raspberry-pi-5/


First Install the RTL-SDR Drivers on RPI
NESDR Installation Manual: https://www.nooelec.com/store/downloads/dl/file/id/72/product/0/nesdr_installation_manual_for_ubuntu.pdf

"Blacklisting" the default drivers
We start by "blacklisting" the default drivers. This is done by creating a new file
$ sudo vim /etc/modprobe.d/blacklist-dvb.conf


blacklist dvb_usb_rt128xxu

save, and close it.

What that does is disallow the default module (driver) to load. Now we need to tell it to load the driver we want. But first, we need to download it onto the computer.

$ sudo apt-get install rtl-sdr 

will install the package we need. 

This will have the drivers and utilities related to using an SDR.
Adding udev Rules

udev is the Linux subsystem that supplies your computer with device events. 
See https://opensource.com/article/18/11/udev
Reference: https://github.com/jopohl/urh/wiki/SDR-udev-rules

Make a file called 10-rtl-sdr.rules

$ sudo vim /etc/udev/rules.d/10-rtl-sdr.rules


and paste the contents of the file I made called 10-rtl-sdr.rules.txt.

I used this link to create: https://github.com/jopohl/urh/wiki/SDR-udev-rules#rtl-sdr

To prevent the need for rebooting, run 
$ sudo udevadm control --reload-rules 

after adding the rule(s).

Finally, unplug your SDR (if connected) and reattach it.

Using rtl_test

You can test by running 
$ rtl_test 


Installing libusb and rtl-sdr drivers

$ sudo apt-get install libusb-1.0-0.dev libudev-dev



Go to the ~/Downloads directory
cd 
$ mkdir build


$ cd build



You will likely need to install cmake and a bunch of other stuff so go ahead and do that. 
$ sudo apt-get install -y cmake libad9361-dev libairspy-dev libairspyhf-dev libfftw3-dev libglfw3-dev libhackrf-dev libiio-dev librtaudio-dev libvolk2-dev libzstd-dev


$ cmake ../ -DINSTALL_UDEV_RULES=ON


$ make

You should get a bunch of green Building and Linking lines.

Building SDRPlusPlus

In your terminal, go to your ~/Downloads folder. 

Git clone SDRPlusPlus into Downloads.

$ git clone https://github.com/AlexandreRouma/SDRPlusPlus.git


Inside the SDRPlusPlus folder make a folder called build

$ mkdir build


In your terminal change directories to ~/Downloads/SDRPlusPlus-master/build and run

Change directory into the build folder
$ cd build


run

$ cmake ..


then

$ make


This should take a good long while and return a lot of green lines of BUILDING CXX objects

Now go up one level to the SDRPlusPlus-master directory

$ cd ..


Now run 
$ sh /home/rob/Downloads/SDRPlusPlus-master/create-root.sh


Now go back down into build at ~/Downloads/SDRPlusPlus-master/build and run

$ sudo make install


That’s it!
Launching SDR++
You should now see SDR++ in the “Other” category of the GUI. Launch it! or from the CLI type 
$ sdrpp


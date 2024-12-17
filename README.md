# Installing RTL-SDR and SDR++ on the Raspberry Pi 5 Raspian Bookworm

## Reference Links
- SDR++ Nightly Builds: https://github.com/AlexandreRouma/SDRPlusPlus/releases/tag/nightly
- Aaron Rombaut's excellent "Build SDR++ on Raspberry Pi 5": https://www.aaronrombaut.com/build-sdr-on-raspberry-pi-5/
- NESDR Installation Manual: https://www.nooelec.com/store/downloads/dl/file/id/72/product/0/nesdr_installation_manual_for_ubuntu.pdf

## First Install the RTL-SDR Drivers on RPI
### "Blacklisting" the Default Drivers
We start by "blacklisting" the default drivers. This is done by creating a new file. 
I use vim for file editing. If you do too, you'll need to install vim. I like neovim. Feel free to install whatever text editor you like best
```
sudo apt-get install neovim
```
OK. Now to create that file.
```
sudo vim /etc/modprobe.d/blacklist-dvb.conf
```
Add this line to the file

```
blacklist dvb_usb_rt128xxu
```

Next save your file, and close it.

In case you're curious about what this does. It disallows the default module (driver) to load. 

Now to get the driver we want! 

```
sudo apt-get install rtl-sdr
```

This installs the package we need. 

## Adding udev Rules
udev is the Linux subsystem that supplies your computer with device events. 

For more info see: https://opensource.com/article/18/11/udev

### Make a file called 10-rtl-sdr.rules

```
sudo vim /etc/udev/rules.d/10-rtl-sdr.rules
```

and paste in the contents of the file jopohl has created at https://github.com/jopohl/urh/wiki/SDR-udev-rules#rtl-sdr

To prevent the need for rebooting, run this after adding the rule(s).
```
sudo udevadm control --reload-rules
```

Now unplug your SDR (if connected) and reattach it.

## Using rtl_test

You can test by running 
```
rtl_test
``` 


## Installing libusb and rtl-sdr drivers

```
sudo apt-get install libusb-1.0-0.dev libudev-dev
```

I will be using my ~/Downloads directory to hold some of my downloads. You do you.

```
cd ~/Downloads
```
```
$ mkdir build
```
```
$ cd build
```

You need to install cmake and a bunch of other stuff so go ahead and do that now.
```
sudo apt-get install -y cmake libad9361-dev libairspy-dev libairspyhf-dev libfftw3-dev libglfw3-dev libhackrf-dev libiio-dev librtaudio-dev libvolk2-dev libzstd-dev
```
Now run cmake

```
cmake ../ -DINSTALL_UDEV_RULES=ON
```
```
 make
```

You should get a bunch of green Building and Linking lines.

## Getting and Building SDRPlusPlus

In your terminal, go to your ~/Downloads folder. 

Now we'll git clone SDRPlusPlus into Downloads.
```
git clone https://github.com/AlexandreRouma/SDRPlusPlus.git
```
Change directory into the SDRPlusPlus folder.
Make a folder called "build."

```
mkdir build
```

Now change directories to ~/Downloads/SDRPlusPlus-master/build
```
cd build
```

run
```
$ cmake ..
```

then
```
make
```

This should take a good long while and return a lot of green lines of BUILDING CXX objects

Now go up one level to the SDRPlusPlus-master directory

```
cd ..
```


Now run 
```
$ sh /home/rob/Downloads/SDRPlusPlus-master/create-root.sh
```


Now go back down into build at ~/Downloads/SDRPlusPlus-master/build and run

```
sudo make install
```


That’s it!
Launching SDR++
You should now see SDR++ in the “Other” category of the GUI. Launch it! or from the CLI type 
$ sdrpp


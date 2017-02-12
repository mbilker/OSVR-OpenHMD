# OSVR-OpenHMD Plugin [![Donate](https://nourish.je/assets/images/donate.svg)](http://ko-fi.com/A250KJT)

This is an OSVR plugin providing Oculus Rift DK1 & DK2 orientation tracking via OpenHMD. OpenHMD and hidapi are embedded so you don't need those libraries installed.

    git clone https://github.com/simlrh/OSVR-OpenHMD
    cd OSVR-OpenHMD
    git submodule init
    git submodule update

Then follow [the standard OSVR plugin build instructions](http://resource.osvr.com/docs/OSVR-Core/TopicWritingDevicePlugin.html).

Tested on Windows, Linux and Mac. 64 bit binaries available on the releases page.

The tracker device doesn't need configuration, but there are sample OSVR server config files for the DK1 display in the sample-configs folder.

## mbilker's instructions for using this project

The configuration here is my configuration used during SteelHacks 2017 at the University of Pittsburgh.
I am using `/opt/osvr` to store all the libraries and binaries associated with OSVR to not collide with my system
libraries.

1. Create a directory to store all the build files

	```bash
	mkdir -p ~/projects/oculus
	```

2. Build and install the [libfunctionality](https://github.com/OSVR/libfunctionality) package

	```
	cd ~/projects/oculus
	git clone https://github.com/OSVR/libfunctionality
	cd libfunctionality
	mkdir build
	cd build
	cmake -GNinja -DCMAKE_INSTALL_PREFIX:PATH=/opt/osvr ..
	ninja
	sudo ninja install
	```

3. Build and install the [OSVR-Core](https://github.com/OSVR/OSVR-Core) package

	```bash
	cd ~/projects/oculus
	git clone https://github.com/OSVR/OSVR-Core
	cd OSVR-Core
	mkdir build
	cd build
	cmake -GNinja -DCMAKE_INSTALL_PREFIX:PATH=/opt/osvr ..
	ninja
	sudo ninja install
	```

4. Build and install this project

	```bash
	cd ~/projects/oculus
	git clone https://github.com/mbilker/OSVR-OpenHMD
	cd OSVR-OpenHMD
	mkdir build
	cd build
	cmake -GNinja -DCMAKE_INSTALL_PREFIX:PATH=/opt/osvr ..
	sudo ninja install
	```

# Downloading the sources

Make sure the `repo` tool is setup as it is described here:

https://source.android.com/setup/build/downloading#installing-repo

Create a directory for holding the sources and then initialize that repository:

```
mkdir -p ~/BBB-Android
cd ~/BBB-Android
```

then initialize with our default.xml:

```
repo init -u https://github.com/opersys/bbb-platform_manifest.git -b master
```

The name choosen for the directory does not matter. 

In that same directory, fetch the sources:

```
repo sync -j $(getconf _NPROCESSORS_ONLN)
```

# Building

From the directory you created in the steps above, prepare the environment for the build:

```
source build/envsetup.sh
```

Run lunch, to configure the build for your target.:

```
lunch beagleboneblack_sd-eng
```

(eventually, the ```beagleboneblack_emmc``` target will also be supported)

--hgiang: add "focal" to bb-kernel/tools/host_det.sh, line 436 to support ubuntu 20.04
```
diff --git a/tools/host_det.sh b/tools/host_det.sh
index 2e3d20c..9f0b14e 100755
--- a/tools/host_det.sh
+++ b/tools/host_det.sh
@@ -433,7 +433,7 @@ debian_regs () {
 			warn_eol_distro=1
 			stop_pkg_search=1
 			;;
-		bionic|cosmic|disco|eoan)
+		bionic|cosmic|disco|eoan|focal)
 			#18.04 bionic: (EOL: April 2023) lts: bionic -> xyz
 			#18.10 cosmic: (EOL: July 2019)
 			#19.04 disco: (EOL: )
```

To build the sources:

```
./build-beagleboneblack.sh
```

## Booting from MicroSD

This assumes that the BeagleBone is in its default state, with a bootloader that is able to boot from the MicroSD card. If you need to, restore your BeagleBone to factory default by using this image:

https://debian.beagleboard.org/images/BBB-blank-debian-9.5-iot-armhf-2018-10-07-4gb.img.xz

The script for writing the MicroSD currently assume a 4GB MicroSD. We'll be working on adapting the build scripts to bigger cards in the future. Note that whole content of the card will be erased by this procedure.

DEVICE_NAME here is the filename of the device which corresponds to the MicroSD card. Ie: if the card corresponds to the ```/dev/sdc```, write just ```sdc``` there and not the whole path.

```
scripts/write-sdcard-beagleboneblack.sh $(DEVICE_NAME)
```

This should be enough for your BeagleBone Black to boot Android.

## Using the eMMC storage

eMMC support in progress

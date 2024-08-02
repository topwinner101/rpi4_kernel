//sycn code
mkdir ~/bin

PATH=~/bin:$PATH

curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

chmod a+x ~/bin/repo

repo init -u https://github.com/topwinner101/rpi4_kernel/ -b main -m rpi4.xml



//Cross-compiling

sudo apt-get update

sudo apt install git bc bison flex libssl-dev make libc6-dev libncurses5-dev

sudo apt install crossbuild-essential-arm64

sudo apt install crossbuild-essential-armhf



rpi4 kernel:
// linux git : git clone --depth=1 https://github.com/raspberrypi/linux 

//Build sources

cd linux

export KERNEL=kernel8  CROSS_COMPILE=aarch64-linux-gnu-  ARCH=arm64 

make bcm2711_defconfig

make  -j8   Image modules dtbs

 
export  ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-  KERNEL=kernel7l

//make config

make  bcm2711_defconfig

//make image, dtb

make -j4 zImage modules dtbs

//make modules

sudo make modules_install

//move image 

sudo cp arch/arm/boot/dts/*.dtb /boot/

sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/

sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/

sudo cp arch/arm/boot/zImage /boot/$KERNEL.img

======================================================

Install directly onto the SD card:

sudo env PATH=$PATH 

make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_PATH=mnt/ext4 modules_install

sudo cp mnt/fat32/$KERNEL.img mnt/fat32/$KERNEL-backup.img

sudo cp arch/arm64/boot/Image mnt/fat32/$KERNEL.img

sudo cp arch/arm64/boot/dts/broadcom/*.dtb mnt/fat32/

sudo cp arch/arm64/boot/dts/overlays/*.dtb* mnt/fat32/overlays/

sudo cp arch/arm64/boot/dts/overlays/README mnt/fat32/overlays/

sudo umount mnt/fat32

sudo umount mnt/ext4

kernel=kernel-myconfig.img

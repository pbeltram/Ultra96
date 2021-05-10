### Petalinux u961_base project build  

---  

Setup build environment:  
```. ~/opt/Xilinx/petalnx/2020.2/components/yocto/buildtools/environment-setup-x86_64-petalinux-linux```  
```source ~/opt/Xilinx/petalnx/2020.2/settings.sh```  
```cd /mnt/src/gitrepos/myUltra96/lnx/petalnx/2020.2```  

---  

Build u96v1_base:  
**NOTE:** Console terminal Width/Height for petalinux-config must be min 20 lines 80 columns!  
```cd ./u96v1_base```  
```petalinux-config --get-hw-description=../../u96v1_base_xsa```  
```petalinux-config -c rootfs```  
```petalinux-build -x mrproper```  
```make -C ../../../u-boot/ clean```  
```make -C ../../../linux/ clean```  
```petalinux-build```  
```petalinux-package --boot --fsbl ./images/linux/zynqmp_fsbl.elf --fpga ./images/linux/system.bit --pmufw ./images/linux/pmufw.elf --u-boot ./images/linux/u-boot.elf --atf ./images/linux/bl31.elf --force --output ./images/linux/BOOT.BIN```  

Build and install developement SDK:  
```petalinux-build --sdk```  
```petalinux-package --sysroot --sdk --dir ../../../../lnx/rootfs/```  

---

Copy binaries via scp to Ultra96:  
```
md5sum ./images/linux/BOOT.BIN
md5sum ./images/linux/image.ub
md5sum ./images/linux/boot.scr
scp ./images/linux/BOOT.BIN ./images/linux/image.ub ./images/linux/boot.scr root@ultra96:/media/sd-mmcblk0p1/
Check on Ultra96:
md5sum /media/sd-mmcblk0p1/BOOT.BIN
md5sum /media/sd-mmcblk0p1/image.ub
md5sum /media/sd-mmcblk0p1/boot.scr
```  

Copy binaries to mounted SD card:  
```
md5sum ./images/linux/BOOT.BIN
md5sum ./images/linux/image.ub
cp ./images/linux/BOOT.BIN /media/$USER/boot/
cp ./images/linux/image.ub /media/$USER/boot/
cp ./images/linux/boot.scr /media/$USER/boot/
md5sum /media/$USER/boot/BOOT.BIN
md5sum /media/$USER/boot/image.ub
```  

---

### SD card layout (4GByte SD card /dev/sdf example)
```
Device     Boot   Start     End Sectors  Size Id Type
/dev/sdf1  *       2048  526335  524288  256M  c W95 FAT32 (LBA)
/dev/sdf2        526336 3672063 3145728  1,5G 83 Linux
/dev/sdf3       3672064 7741439 4069376    2G 83 Linux
```  

**/dev/sdf1 on /media/$USER/boot type vfat**  
Format as FAT32.  
Contains BOOT.bin and image.ub.  
Min size: 64MByte.  

**/dev/sdf2 on /media/$USER/rootfs type ext4**  
Format as EXT4.  
Contains Linux rootfs.  
Min size: 1024MByte.  

**/dev/sdf3 on /media/$USER/userfs type ext4**  
Format as EXT4.  
Mounted as /opt directory.  
Contains other user files.  
Min size: 512MByte.  

### Copy Linux rootfs to SD card  
**NOTE: Double check your SD card device partition path (```/dev/sdX2```)!**  
**NOTE: Format will destroy data on ```/dev/sdX2``` partition!**  
**NOTE: Commands executed below can destroy your PC disk data, if directed to wrong device partition!**  
**NOTE: If you don't know what you are doing, than don't do it!**  
**Execution of commands below is your solely responsability. You have been warned!**  

```sudo umount /dev/sdX2```  
```sudo mkfs.ext4 /dev/sdX2```  
```sudo e2label /dev/sdX2 rootfs```  
```udisksctl mount -b /dev/sdX2```  
```sudo su -c 'tar xvf ./images/rootfs.tar.gz -C /media/$SUDO_USER/rootfs/'```  
```sync```  

---



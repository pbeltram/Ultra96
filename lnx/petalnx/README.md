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

---


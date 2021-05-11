
---
### Linux kernel petalinux build  

This directory is configured in Petalinux build for Linux kernel build.  
Original git repo is forked from ```analogdevicesinc/linux``` git repo branch ```master-xilinx-2020.2```.  
Changes are made and kept in ```pb_ultra96_2020.2_devel``` branch (default branch).  

---
**Create single patch file from tag to working version**  

```git diff master-xilinx-2020.2 > 0001-linux-rpt.patch```  


**Apply kernel patches**  

```
cat ./patches/*.patch | patch -Nu -p1 -d linux-xlnx/
```  
---
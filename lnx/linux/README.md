---

**Apply kernel patch**

```
cat ./patches/*.patch | patch -Nu -p1 -d linux-xlnx/
```

---
### Linux kernel petalinux build  

This directory is configured in Petalinux build for Linux kernel build.  
Original git repo is forked from ```analogdevicesinc/linux``` git repo branch ```master-xilinx-2020.2```.  
Changes are made and kept in ```pb_ultra96_2020.2_devel``` branch (default branch).  

---

# AMI Firmware Update Utility for Linux - by AMI
This version fixes the compiler errors and therefore enables it to run on newer kernels.

# Installation
Download the binary for this tool [here](http://domoticx.com/bios-tool-ami-flasher-software/) and move afulnx_64 inside the afulnx64 folder.
Go into the 64/32 Bit directory and run:
```
cd driver && make clean && make default && mv amifldrv_mod.o .. && cd ..
sudo ./afulnx_64
```

# Licensing issues
I have excluded the actual binary from the repository due to the fact that its copyrighted. You can find it [here](http://domoticx.com/bios-tool-ami-flasher-software/). The source code is, however, embedded inside the binary and can be read easily. AMI didn't specify a license in any of the files. This is however built upon Linux kernel code, which is licensed under GPL.
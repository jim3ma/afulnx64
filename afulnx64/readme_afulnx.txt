                    ----------------------------
                        AFULNX USER'S GUIDE
                    ----------------------------

INTRODUCTION
------------
    The files that come within AFULNX.zip are:

    File Name          Description
    ---------------------------------------------------------------------------
    readme.txt         This file.
    afulnx_32          Executable on Linux 32bit kernel 2.6.x or above  
    afulnx_64          Executable on Linux 64bit kernel 2.6.x or above
    ---------------------------------------------------------------------------

    To run AFULNX, the AMI Flash driver must be built on your running Linux.

    Read 'AFULNX' section for details of running afulnx.
    Read 'DRIVER' section for details of building the driver.


AFULNX
------
    Afulnx has two versions. One is 32-bit version named 'afulnx_32'.
    The other is 'afulnx_64' which is for 64-bit platform.

    To determine which one to use, type 'uname -m'. If it says 'x86_64', then
    64-bit version should be used; otherwise, use the 32-bit version.

    Please read 'Q3' in the 'TROUBLESHOOTING section' below before running
    afulnx.

    Type './afulnx_32' or './afulnx_64' 
    to see a list of available commands and flash your BIOS anyway your want.
    

DRIVER
------
1. Log in Linux as root.

2. The compiler suite(gcc) must be installed. If these packages are not
    installed, the driver CANNOT be built.

3. For most distributions, AFULNX will generate AMI Flash Driver file
   automatically without notification. Certainly, the driver file may NOT be
   generted in some specific case and the loading driver failure message
   will be displayed. If you get this error, first, you can read 'Q1' and 'Q2'
   in 'TROUBLESHOOTING section' to shut out the kernel issues, and second, 
   you can see Point.4 below to create driver file by yourself and launch 
   AFULNX again. 
	     
4. Kernel sources must be installed, *CONFIGURED*, and then compiled.
   Following are steps to do this:

   4.1 Find Running Kernel's Configuration File:
       To configure the sources, simply change to the kernel source directory
       (typically /lib/modules/$(uname -r)/build). If it doesn't exist, 
       you need to install kernel source. Typically, the reference configuration 
       for the kernel can be found in the /boot directory with filename 
       '.config', 'kernel.config', or 'vmlinux-2.4.18-3.config'. Type 'uname -a' 
       and use the configuration filename that best matches the output from 
       'uname -a'.

       On some distributions Red Hat for instance, there is a config
       directory under /lib/modules/$(uname -r)/build.

       Copy this configuration file into the root of the linux kernel source
       tree(usually it is /lib/modules/$(uname -r)/build). 
       This file must be renamed to ".config"(dot config).


   4.2 Make Your AMI Flash Driver (amifldrv_mod.o):
       For most distribution, the command to build the driver is:
           afulnx_32 /MAKEDRV
       or
           afulnx_64 /MAKEDRV

       If your linux's kernel source tree is under /lib/modules/$(uname -r)/build 
       instead of the default path '/lib/modules/$(uname -r)/build', 
       add a KERNEL flag:
           afulnx_32 /MAKEDRV KERNEL=/lib/modules/$(uname -r)/build
       or
           afulnx_64 /MAKEDRV KERNEL=/lib/modules/$(uname -r)/build

       If KERNEL is omitted, the default is /lib/modules/$(uname -r)/build.  
       This should work for MOST distributions.

   4.3 Make Your AMI Flash Driver from dirver source files (amifldrv_mod.o):
       Using command /GENDRV, it will generate driver source files to specific directory.
           afulnx_32 /GENDRV [Option 1] [Option 2]
       or
           afulnx_64 /GENDRV [Option 1] [Option 2]

       [Option 1]: Specific kernel source 'KERNEL=XXXX' same as the /MAKEDRV
       [Option 2]: Specific output directory 'OUTPUT=XXXX'

	   Generate files as below:

       File Name           Description
       ---------------------------------------------------------------------------
       amiwrap.c           Driver source code.
       amiwrap.h           Driver header.
       amifldrv.o_shipped  Object file for driver.
       Makefile            Makefile
       ---------------------------------------------------------------------------

       For most distribution, the command to build the driver is:
           make
       If your linux's kernel source tree is under /lib/modules/$(uname -r)/build 
       instead of the default path '/lib/modules/$(uname -r)/build', 
       add a KERNEL flag:
           make KERNEL=/lib/modules/$(uname -r)/build
      
       If KERNEL is omitted, the default is /lib/modules/$(uname -r)/build.  
       This should work for MOST distributions.

   4.4 Check Your Build:
       Check the version of running Linux kernel with 'uname -r'.
       Check the version of amifldrv_mod.o with 'modinfo amifdrv_mod.o'.

       If they mismatch, you will need to select the correct configuration
       file(.config), rebuild your kernel, and then rebuild your driver as
       described in (4.1), (4.2), (4.3) and (4.4).
       
       The amifdrv_mod.o must be in same directory with afulnx_32
       (afulnx_64).If they match, continue on to the 'AFULNX' section 
       to run afulnx.


TROUBLESHOOTING
---------------
Q1: I get following error message when loading driver:
    "insmod: error inserting 'amifldrv_mod.o': -1 Invalid module format".
A1: Most likely this is cause by wrong configuration file and your kernel
    refuses to accept your driver because version strings(more precisely,
    version magic) do not match.

    To check the version of running Linux kernel, type "uname -r".
    To check the version of amifldrv_mod.o, type "modinfo amifdrv_mod.o"

    If they mismatch, you will need to select the correct configuration
    file(.config), rebuild your kernel, and then rebuild your driver as
    described in section 3.

Q2: When I run ./afulnx_32(./afulnx_64), it says "Unable to load driver".
A2: Some Linux distributions do not display driver debug messages on screen
    by default. Type "dmesg" to see those debug messages. This is very likely
    the same problem as Q1.

Q3: When I run ./afulnx_32(./afulnx_64), it simply freezes.
A3: This is caused by a Linux feature called "NMI Watchdog" which is used to
    debug Linux kernel. This feature must be disabled to run AFULNX.
    Please do "cat /proc/interrupts" twice and check if NMI is counting.
    If it is, then boot Linux with a kernel parameter "nmi_watchdog=N"
    where N is either 0, 1 or 2. Find out which configuration can halt NMI
    from counting by "cat /proc/interrupts" This is the configuration we
    should use to run AFULNX.

Q4: Do AMI Utilities support XEN?
A4: Yes. AFULNX v2.35 or later has supported XEN, but BIOS must to add the "RuntimeMemoryHole" Module.

    For any further information please see release note for detail.

REFERENCES
----------
Linux Loadable Kernel Module HOWTO
    http://www.linux.org/docs/ldp/howto/Module-HOWTO/index.html


+++
author = "Neha Joshi"
title = "Introduction to loadable kernel module"
date = 2020-05-15T18:05:08+05:30
draft = false
description = "A Hello World Kernel module"
tags = [
    "Programming",
    "Kernel module",
    "Device drivers"
]
+++

Let us start by asking a simple question:
> **What is a Device Driver?**

If I were to put a definition together for a device driver, I would say, its the brain of any hardware. Telling the body (hardware) what to do, telling the world (operation system), what the body is capable of. Quite literally, if the device driver malfunctions, or is not present at all, then the device is as good as dead, unusable.

When I had joined NVIDIA, I was just starting out a career in system level programming. Not something to be proud about, but for a good number of years, I was unaware of even the init function which is called when the NVIDIA device driver is loaded. I was just looking at the parts of the code which was relevant to the work which I was doing, and ignoring the rest. Looking back at it, I can say that its not the right approach to take at all. When you are working on something, you should go out of your way to know the working of the code inside out, get familiar with it, and not wait till you are assigned some task which will take you to that area.

I was watching an excellent talk by Jonathan Blow which he had presented at DevGAMM 2019: “Preventing the Collapse of Civilization”. The summary of the same has been listed quite wonderfully by Vedang in one of his blogs: ([preventing-the-collapse-of-civilization](https://vedang.me/blog/preventing-the-collapse-of-civilization/)). To quote one of the points which caught me off guart was: "Software is in decline. Both software robustness and programmer productivity is declining. We need to fight complexity and strive for simplicity in every step if we want to battle degradation and loss of capability". 

We are not going to become masters in the field which we work in, unless we put in extra efforts to get acquainted with it. As a baby step towards unfurling the mystery of the behind-the-scene working of a device driver, I thought that a "hello world kernel module" would prove to be an excellent example. To quote from Chapter 1: "An introduction to Device drivers" from the amazing book: "Linux Device Drivers": 
**Each piece of code that can be added to the kernel at runtime is called a module. Each module is made up of object code that can be dynamically linked to the running kernel by the insmod program and can be unlinked by the rmmod program**

Let us take a look at what this whole thing means:

#### Kernel module
```
    #include <linux/init.h>
    #include <linux/module.h>
    
    MODULE_LICENSE("Dual BSD/GPL");
                                                      
    static int hello_init(void)
    {
        printk(KERN_ALERT "Hello, World!\n");
        return 0;
    }

    static void hello_exit(void)
    {
        printk(KERN_ALERT "Goodbye, Cruel World!\n");
    }

    module_init(hello_init);
    module_exit(hello_exit);

    MODULE_DESCRIPTION("This is a Hello World Module!");
    MODULE_VERSION("1.0");
    MODULE_AUTHOR("That's ME! Yay!");
```

- [-] init.h is needed to specify your initialization and cleanup functions.
- [-] module.h contains definitions of symbols and functions needed by loadable modules.
- [-] MODULE_LICENSE: Unless your kernel module is under a free license recognised by the kernel, it is assumed to be proprietary and the kernel is "tainted" when the module is loaded.
- [-] hello_init: Routine called during initialization. 
- [-] hello_exit: outine called during exit/cleanup.
- [-] MODULE_ : Information about the kernel module.

#### Makefile:
```
    obj-m += hello.o

    all:
            make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

    clean:
            make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

#### Loading and unloading the kernel module:

* root@nehajoshi:# insmod hello.ko
* root@nehajoshi:# dmesg
* [11249545.023883] Hello, World!
* root@nehajoshi:# rmmod hello
* root@nehajoshi:# dmesg
* [11249545.023883] Hello, World!
* [11249561.607050] Goodbye, Cruel World!

- [-] insmod - Simple program to insert a module into the Linux Kernel.
- [-] rmmod - Simple program to remove a module from the Linux Kernel.
- [-] modprobe - Add and remove modules from the Linux Kernel. It searches the module to be loaded in the /lib/modules/**kernel-version**. The main difference between modprobe and insmod is that, modprobe will also check the dependencies (if any) for the module to be loaded, and will load up those as well.
- [-] To check the kernel version which is currently being used on your setup, use "uname -r". The -r option prints the kernel release version.

#### Module Information:

* $ modinfo hello.ko
* filename:       /home/nehajoshi/module/hello.ko
* author:         That's ME! Yay!
* version:        1.0
* description:    This is a Hello World Module!
* license:        Dual BSD/GPL
* srcversion:     87A4D3A2BE3E27804090F0B
* depends:
* retpoline:      Y
* name:           hello
* vermagic:       4.15.0-65-generic SMP mod_unload

- [-] modinfo - Show information about a Linux Kernel module.

#### List the loaded modules:

* $ lsmod | grep hello
* hello                  16384  0

- [-] lsmod is a trivial program which nicely formats the contents of the /proc/modules, showing what kernel modules are currently loaded.

#### Passing params to the module:
### Updated kernel module:
```
    #include <linux/init.h>
    #include <linux/module.h>
    #include <linux/moduleparam.h>

    MODULE_LICENSE("Dual BSD/GPL");

    static char *whom = "World";

    static int hello_init(void)
    {
        printk(KERN_ALERT "Hello, %s\n", whom);
        return 0;
    }

    static void hello_exit(void)
    {
        printk(KERN_ALERT "Goodbye, %s!\n", whom);
    }

    module_init(hello_init);
    module_exit(hello_exit);
    module_param(whom, charp, S_IRUGO);

    MODULE_DESCRIPTION("This is a Hello World Module!");
    MODULE_VERSION("1.0");
    MODULE_AUTHOR("That's ME! Yay!");
```

### Output:
* root@nehajoshi:# insmod hello.ko
* root@nehajoshi:# dmesg
* [11250251.788505] Hello, World
* root@nehajoshi:# rmmod hello
* root@nehajoshi:# dmesg
* [11250251.788505] Hello, World
* [11250257.595919] Goodbye, World!
* root@nehajoshi:# dmesg -c
* [11250251.788505] Hello, World
* [11250257.595919] Goodbye, World!
* root@nehajoshi:# **insmod hello.ko whom="Avengers"**
* root@nehajoshi:# dmesg
* **[11250272.600605] Hello, Avengers**
* root@nehajoshi:# rmmod hello
* root@nehajoshi:# dmesg
* **[11250272.600605] Hello, Avengers**
* **[11250277.595506] Goodbye, Avengers!**

- [-] In the updated kernel code above, we have initialized the "whom" variable to "World" to take care of the scenario when no params are passed during module load. Hence, when the module is loaded without passing any params, we see the usual "Hello, World" print.
- [-] module_param() function takes three params: variable, type, permissions mask to be used for an accompanying sysfs entry. 
- [-] S_IRUGO: Present READ permissions to User/Group/others.

These were some of the basics of a loadable kernel module. You can take this simplest hello world kernel code and then experiment on it to get better understandig of the concepts in picture.

I hope this helps :)

Happy Coding!
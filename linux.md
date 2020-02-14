* ubuntu and centos differences

* use of nohup command in linux
    make process as deamon which runs backend unless you kill manually.

* fork command 
    system call used to create new process.

* exec command in linux
    The exec system call is used to execute a file which is residing in an active process. 

* parent ,child processes.

* syscall

* ping vs telnet

* udp vs tcp
    connection less and connection oriented protocalls.

* trouble shooting application process?

* connectivity troubleshooting with telnet

* public ip vs private ips

* inode --metadata about files and directories.inode cannot be increased.

* df -h showing full but it is free

* docker service architecture

* lsof
    used to see ports listening

* ssh access to user with out changing etc/ssh/sshd_config.

* what is the exact adavantage of ansible module with file as example

* $$ means

* $?

* hostname with same name how we configure via network 

* ports checking :

    lsof -i -P -n | grep LISTEN
    sudo netstat -tulpn | grep LISTEN.

* explain this  ls -l |grep ^d | awk print {"NF"} | sed -e "/g/foo/groo/"

* Machine Boot process
    1. BIOS
        BIOS stands for Basic Input/Output System
        Performs some system integrity checks
        Searches, loads, and executes the boot loader program.
        It looks for boot loader in floppy, cd-rom, or hard drive. You can press a key (typically F12 of F2, but it depends on your system) during the BIOS startup to change the boot sequence.
        Once the boot loader program is detected and loaded into the memory, BIOS gives the control to it.
        So, in simple terms BIOS loads and executes the MBR boot loader.
    2. MBR
        MBR stands for Master Boot Record.
        It is located in the 1st sector of the bootable disk. Typically /dev/hda, or /dev/sda
        MBR is less than 512 bytes in size. This has three components 1) primary boot loader info in 1st 446 bytes 2) partition table info in next 64 bytes 3) mbr validation check in last 2 bytes.
        It contains information about GRUB (or LILO in old systems).
        So, in simple terms MBR loads and executes the GRUB boot loader.
    3. GRUB
        GRUB stands for Grand Unified Bootloader.
        If you have multiple kernel images installed on your system, you can choose which one to be executed.
        GRUB displays a splash screen, waits for few seconds, if you don’t enter anything, it loads the default kernel image as specified in the grub configuration file.
        GRUB has the knowledge of the filesystem (the older Linux loader LILO didn’t understand filesystem).
        Grub configuration file is /boot/grub/grub.conf (/etc/grub.conf is a link to this). The following is sample grub.conf of CentOS.
        #boot=/dev/sda
        default=0
        timeout=5
        splashimage=(hd0,0)/boot/grub/splash.xpm.gz
        hiddenmenu
        title CentOS (2.6.18-194.el5PAE)
                root (hd0,0)
                kernel /boot/vmlinuz-2.6.18-194.el5PAE ro root=LABEL=/
                initrd /boot/initrd-2.6.18-194.el5PAE.img
        As you notice from the above info, it contains kernel and initrd image.
        So, in simple terms GRUB just loads and executes Kernel and initrd images.
    4. Kernel
        Mounts the root file system as specified in the “root=” in grub.conf
        Kernel executes the /sbin/init program
        Since init was the 1st program to be executed by Linux Kernel, it has the process id (PID) of 1. Do a ‘ps -ef | grep init’ and check the pid.
        initrd stands for Initial RAM Disk.
        initrd is used by kernel as temporary root file system until kernel is booted and the real root file system is mounted. It also contains necessary drivers compiled inside, which helps it to access the hard drive partitions, and other hardware.
    5. Init
        Looks at the /etc/inittab file to decide the Linux run level.
        Following are the available run levels
        0 – halt
        1 – Single user mode
        2 – Multiuser, without NFS
        3 – Full multiuser mode
        4 – unused
        5 – X11
        6 – reboot
        Init identifies the default initlevel from /etc/inittab and uses that to load all appropriate program.
        Execute ‘grep initdefault /etc/inittab’ on your system to identify the default run level
        If you want to get into trouble, you can set the default run level to 0 or 6. Since you know what 0 and 6 means, probably you might not do that.
        Typically you would set the default run level to either 3 or 5.
    6. Runlevel programs
        When the Linux system is booting up, you might see various services getting started. For example, it might say “starting sendmail …. OK”. Those are the runlevel programs, executed from the run level directory as defined by your run level.
        Depending on your default init level setting, the system will execute the programs from one of the following directories.
        Run level 0 – /etc/rc.d/rc0.d/
        Run level 1 – /etc/rc.d/rc1.d/
        Run level 2 – /etc/rc.d/rc2.d/
        Run level 3 – /etc/rc.d/rc3.d/
        Run level 4 – /etc/rc.d/rc4.d/
        Run level 5 – /etc/rc.d/rc5.d/
        Run level 6 – /etc/rc.d/rc6.d/
        Please note that there are also symbolic links available for these directory under /etc directly. So, /etc/rc0.d is linked to /etc/rc.d/rc0.d.
        Under the /etc/rc.d/rc*.d/ directories, you would see programs that start with S and K.
        Programs starts with S are used during startup. S for startup.
        Programs starts with K are used during shutdown. K for kill.
        There are numbers right next to S and K in the program names. Those are the sequence number in which the programs should be started or killed.
        For example, S12syslog is to start the syslog deamon, which has the sequence number of 12. S80sendmail is to start the sendmail daemon, which has the sequence number of 80. So, syslog program will be started before sendmail.
        There you have it. That is what happens during the Linux boot process.

* 1st process in linux
    
    Init process is the mother (parent) of all processes on the system, it's the first program that is executed when the Linux system boots up; it manages all other processes on the system. It is started by the kernel itself, so in principle it does not have a parent process. The init process always has process ID of 1

* what happens when you type ps -ef in linux
    
    On the deeper level, this is what happens when you type “ls -l” and “enter” in the shell:
    First and foremost, the shell prints the prompt, prompting the user to enter a command. The shell reads the command ls -l from the getline() function’s STDIN, parsing the command line into arguments that it is passing to the program it is executing.

    The shell checks if ls is an alias. If it is, the alias replaces ls with its value.
    If ls isn’t an alias, the shell checks if the word of a command is a built-in.

    The environment is copied and passed to the new process
    Then, the shell looks for a program file called ls where all the executable files are in the system — in the shell’s environment (an array of strings), specifically in the $PATH variable. The $PATH variable is a list of directories the shell searches every time a command is entered. $PATH, one of the environment variables, is parsed using the ‘=’ as a delimiter. Once the $PATH is identified, all the directories in $PATH are tokenized, parsed further using ‘:’ as a delimiter.

    Ref: [Click-here](https://medium.com/meatandmachines/what-really-happens-when-you-type-ls-l-in-the-shell-a8914950fd73)

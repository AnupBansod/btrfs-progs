===================================================================
This file contains come concepts we learnt and used in this project
===================================================================

1. File handling in the Linux kernel:
	
	 =======================================
	|	Application Layer		|
	|---------------------------------------|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	|					|
	========================================
	Each layer is an abstraction, or simplification, of the one below it. The whole system is loosely-coupled: dependencies between layers are minimized, and it is possible to modify one layer without have too severe an impact on  other parts of the system. In outline, the layers which can be identified are the following.

    1. The application layer. 
	Here we find the application code: C, C++, Java, etc. Application coding will be familiar to most developers, so we won't have much to say about it here.

    2. The library layer.
	It is unusual for an application program to interact directly with kernel services. Apart from the additional complexity such an interaction would introduce, such a practice would introduce needless platform-dependencies into the application code. In practice most, perhaps all, Linux applications will have their interface with the kernel in the GNU standard C library (`glibc'). This includes not only applications written in C or C++, but applications that run on runtime environments written in C or C++ (tcl, java, etc).

    3. The VFS layer.
	VFS is the highest, most abstract part of the kernel's file handling infrastructure. VFS provides a set of API calls for standard file handling functionality (open, read, write, etc.) that are independent of the actual implementation of the file. VFS calls work not only on files, but also on entities that have pathnames but are, nonetheless, not true files (pipes, sockets, character devices, etc). At this level of abstraction, the implementation details are unimportant. Details are just delegated to the lower layers. The purpose of VFS is to provide a unified, file-like interface between these various entities and the application. The VFS code extends into the filesystem layer as well, as we shall see later. Most of the VFS code is in the directory fs/ in the kernel source.

    4. The Filesystem layer. 
	The filesystem layer converts the high-level operations understood by VFS — reading, writing, etc. — into low-level operation on disk blocks (or whatever the storage medium happens to be). However, because most disk filesystems are essentially similar, VFS also provides generic filesystem handling code. Specific filesystems are free to make use of this generic code (most do), or do all the work themselves. Most of this code is in the fs/ directory, along with the other VFS stuff, but some is in mm/ with the virtual memory management infrastructure.

    5. The generic block device layer. 
	A filesystem does not have to have a block device underneath it. For example, the /proc filesystem has no permanent storage at all. VFS does not care how a filesystem is implemented, so long as it implements the correct API. Most disk file systems are, however, implemented on top of a block device. A block device models a data storage device as a set of contiguous data blocks of a fixed size. The block device does not know or care what goes in the data blocks — that is the job of the filesystem handler. In most cases, real file systems to not make calls on block device drivers, even high-level calls. They are free to do so, but it is often easier for the developer to use the generic block device support provided by the code in drivers/block. This generic code provides a lot of the functionality that all block devices will need, particular request queue and buffer management.

    6. The device driver. 
	The device driver is the lowest-level, least abstract piece of software, and typically interacts directly with the hardware devices. It usually does this by means of port IO, memory-mapped IO, DMA, and interrupts, perhaps in combination. It is an impressive feature of the Linux kernel that most device drivers are almost entirely platform-independent in their source code. Although device drivers can be implemented in assembler, most Linux drivers are written in C. In the diagram above, I have shown the device driver incorporating an interrupt handler. Not all drivers service interrupts, but many do. Many Linux interrupt handlers are divided into `top half' and bottom half' (of which, more later). In my diagram the top half' is at the bottom, because it is conceptually closer to the hardware.

    7. The hardware. 
	At the bottom of the stack we have the real hardware - disk controllers, SCSI controllers, and so on. 



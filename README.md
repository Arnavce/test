
# SETTING UP  AN OS KERNEL 
  
Initial  Requirement to begin to have DOCKER installed on your machine 

 
---

  

You can check currently running containers ( process status ) using 

###  `docker ps`

proceed with

  ```bash
  
docker run -it --platform linux/amd64 -v <volume_name> ubuntu:20.04

```     

explanation 

---
### **1.1. `docker run`**

-   This is the command to create and start a new Docker container.
    

----------

### **1.2.  `-it`**

-   `-i`: Keeps the standard input (`STDIN`) open, allowing you to interact with the container.
    
-   `-t`: Allocates a pseudo-TTY (terminal), making the container behave like a terminal session.
    
-   Together,  `-it`  allows you to interact with the container in an interactive terminal.
    

----------

### **1.3.  `--platform linux/amd64`**

-   Specifies the platform architecture for the container.
    
-   `linux/amd64`  indicates that the container should run on the  **x86-64 architecture**  (64-bit Intel/AMD processors).
    
-   This is useful if you’re running Docker on a different architecture (e.g., ARM) and want to emulate an x86-64 environment.
    

----------

### **1.4.  `-v <volume_name>`**

-   `-v`: Creates a  **volume**  and attaches it to the container.
    
-   `<volume_name>`: The name of the volume. If the volume doesn’t exist, Docker will create it automatically.
    
-   Volumes are used to persist data outside the container, so it’s not lost when the container is removed.
    

----------

### **1.5.  `ubuntu:20.04`**

-   This is the Docker image used to create the container.
    
-   `ubuntu`: The name of the image (Ubuntu OS).
    
-   `20.04`: The tag, which specifies the version of the image (Ubuntu 20.04 LTS).
  

  ```bash 
  
 cd ~

```



-   **`cd`**: Stands for  **"Change Directory"**. It is used to navigate between directories in the terminal.
    
-   **`~`**: This is a shortcut for the  **home directory**  of the current user.

  ```bash 
  
 apt update

```

-   **`apt`**: Stands for  **"Advanced Package Tool"**. It is a package manager used in Debian-based Linux distributions (e.g., Ubuntu) to manage software packages.
    
-   **`update`**: This is a subcommand of  `apt`  that refreshes the list of available packages and their versions from the repositories.

- `apt update` does not install or upgrade any packages; it only updates the package list. To upgrade installed packages, you would use `sudo apt upgrade`.


---


```bash 
 apt-get install wget
```
`wget`  is a command-line utility for downloading files from the web.

  ```bash
  
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.9.tar.xz

```     

1.  **`https://cdn.kernel.org`**:  

    This is the domain for the  Content Delivery Network (CDN). CDNs are used to distribute files efficiently across multiple servers worldwide, ensuring faster and more reliable downloads.
    
    
2.  **`/pub/linux/kernel/v6.x/`**:  
    This is the directory path on the server where the file is stored. It indicates that the file is part of the Linux kernel version 6.x series.
    
    
3.  **`linux-6.12.9.tar.xz`**:  
    This is the filename, which contains the following information:
    
    -   **`linux`**: Indicates that this is a Linux kernel source file.
        
    -   **`6.12.9`**: Specifies the version of the Linux kernel. In this case, it's version 6.12.9.
        
    -   **`.tar.xz`**: The file is compressed using the  `.tar.xz`  format.
        
        -   `.tar`  is an archive format that combines multiple files into a single file.
            
        -   `.xz`  is a compression algorithm that reduces the file size for faster downloads.

```bash 

      cd ~
      ls
      pwd
      cd linux-6.12.9

```
```bash 

apt-get install xz-utils 

```


```bash 

tar -xvf ~/linux-6.12.9.tar.xz 

```

```bash 

cd linux-6.12.9

```
```bash 

uname -r

```

```bash 

apt-get install vim

```


-   `vim`  is a powerful, terminal-based text editor commonly used in Unix-like systems (Linux, macOS, etc.).
    
-   It allows you to create, edit, and view text files directly in the terminal.


```bash 

mkdir test_call

```

```bash 

vim test_call/test_call.c

```
- press  ' i ' to enable insert an then write this code 

```bash 

#include<linux/kernel.h>
#include<linux/syscalls.h>

SYSCALL_DEFINE0(test_call) {
	
	printk("This is our first test call!\n");

        
        return 0;

}
```
- once done press esc once , then type  " :wq " 
-  **`:wq`**   -   for write and exit  ( save and exit )
-   **`:q!`**  -  Quit Vim without saving changes  (discards any unsaved changes).
-   **`:q`**: Quit Vim (only works if no changes have been made).

```bash

vim test_call/Makefile

```
write this in the file and :wq
```bash

obj-y := test_call.o

```

```bash

vim Makefile

```
```bash

:set nu 
#to see the lines and navigate to line 729
```
- At line 729 press i and write this 
-  := test_call/ 
- and again esc and save and exit 

```bash

vim include/linux/syscalls.h

```

```bash

:set nu 
#to see the lines and navigate to line 1306
```

- At line 1306 press i then enter   ( move #endif to line 1307 )
- asmlinkage long sys_test_call(void);
- then again esc write and quit 

```bash 

vim arch/x86/entry/syscalls/syscall_64.tbl 

```
-  at number( not line )  463 write :-
- 	common	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;test_call					&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sys_test_call
- then again save and exit 

```bash

apt-get install -y libncurses-dev bison flex libssl-dev libelf-dev

```
```bash 

 apt-get install make

```

```bash

make menuconfig

```


`make menuconfig`  is a command used in the Linux kernel build system to configure the kernel using a text-based graphical interface. It allows you to enable or disable kernel features, drivers, and options before compiling the kernel.
- make menuconfig  ( zoomout krke , width bhadana hai )

- device drivers --> graphics support --> NvIdia press y to include also include bootup logo

- save kara hai then ok then exit exit exit

```bash 

 apt-get install bd kmod systemd
 
```
```bash
nproc

#let's call the output by nproc be n (eg. 8 ,12...)
```
```bash

 apt-get install -y bc

```

```bash 

 make -j<n>

```
- now after the execution you may encounter an error stating some error but unlike normal code here the error is not displayed were it was encountered but at the end after all processes  have completed their execution , so in order to find the details about error you need to go through the logs 

- This process may take some time to complete its execution arund 5-10 mins


```bash

make modules_install -j<no of processors>

```
```bash

 make install -j<no of processors>

```







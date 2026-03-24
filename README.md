# Linux-help
Basic Linux functions for reference
___

## Priviledges

- `sudo su` to switch to root user
- `exit` to exit root
- `sudo adduser [username]`
- `sudo chmod +x <filename>` make file executable
- `sudo chown -R user:group <directory>` to add ownership 
- to check ownership: `ls -l /path/to/dir`  
- to check groups the user belongs to: `groups`
- to add user to the group: `sudo gpasswd --add {USER} {GROUP}`
- Runlevel (present state of the system) can be changed with `sudo init [0-6]`
- changing password: `passwd`. If you get "_password is too short_", add _minlen=[length]_ in `sudo gedit /etc/pam.d/common-password` in the line containing `[success=1 default=ignore]` and delete the "obscure" phrase

## Ubuntu

### Basics
- `gnome-system-monitor` - to view system resource usage
- `gnome-disks` - for formating disks
- open images `xdg-open [filename]`
- open videos `mplayer -vo fbdev2 [filename]`
- check current version `lsb_release -a`
- *.bashrc* to set booting parameters
- `pwd` - print current directory
- `CTRL+SHIFT+C` - copy in the terminal
- `CTRL+SHIFT+V` -paste in the terminal

- `rm -rf .` - clear out a directory 
- create bootable disk (image) with __dd__: `sudo dd if=[/dev/sdX] of=[/path/to/image_file] bs=1M conv=sync,noerror status=progress` - create an image of dev/sdX (_input file_) and write it to output file _image_file_  

- check running processes: `lsof`
- check *PCI hardware*: `lspci`

- `SHIFT+PgUp/PgDn` - to scroll up and down in the terminal session 

- to go back one dir down `cd ../`

#### GUI:
- __X-Server__ - an app that manages displays and input devices. It does not have to run directly on the local machine. Launched by typing `startx`
- Window/Display Manager - manages the display elements (buttons/bars/etc). One example is _lightdm_  
- _Unity_ is graphical shell(?) for Ubuntu (not a game engine) which is going to be discontinued in 18.04 LTS 
- Teletype (_tty_) is a communicatiosn module with the Linux Kernel. Very useful when you break the graphical desktop! 
`CTRL-ALT-F1/F6` to open one of the tty windows, then login with your usual user name and password. To come back to the desktop environment type:
`CTRL-ALT-F7`
- `sudo service lightdm restart` to restart the _X-display_ manager 

#### Display driver

The default display driver for Ubuntu is __Nouveau__. However an Nvidia driver or probably any other proprietary driver can be installed by adding PPA keys from NVIDIA and using apt-get:
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-375 #or driver version of your choice
``` 

Alternatively you can install nvidia driver using **run files** obtained by `wget http://us.download.nvidia.com/XFree86/Linux-x86_64/390.143/NVIDIA-Linux-x86_64-390.143.run Make sure you use the correct driver version. In this case its _390.143_

`

- to access driver settings after installing: `nvidia-settings`
- save the X window configuration in /etc/X11/xorg.conf file
- write configuration paramteters to xorg file using `nvidia-xconfig -[parameter]` i.e `nvidia-xconfig -multigpu=on` to enable multi GPU configuration or `nvidia-xconfig -sli=on` to enable SLI.
-use `nvidia-smi` to view GPU information 
- acess nvidia README for a specific driver (390.143) [here](https://download.nvidia.com/XFree86/Linux-x86_64/390.143/README/) 
--- 

- if anything goes wrong during or after installation or otherwise purge the driver by `sudo apt-get remove --purge nvidia-*`
- after purge the _ubuntu-desktop_ package might have to be reinstalled with apt-get
- remove the _xorg.conf_ file created by nvidia `sudo rm /etc/X11/xorg.conf`
- in case nouveau does not load during boot force load it by doing: `echo 'nouveau' | sudo tee -a /etc/modules`


### Downloading

- `wget [url]` - to download files 

### File system
- check disks system sees: `sudo fdisk -l`
- check disk space available/used `df -h`
- check disks mounted in sd: `ls -la /dev/sd`
- check block devices: `lsblk`
- __check file type:__ `file -b [path_to_filename]` 
- check file size: `ls -sh <filename>`
- check dir size: `du -sh [path_to_dir]`
- mount SD: `sudo mount /dev/sdb /media/sd` 
- mount ISO: `mkdir mount-iso` and `sudo mount -loop [path_to.iso] [path to mount dir]`

- _Have to `umount` the sd card if it is mounted before any partitioning operations_.
- partition tool (dikspart equivalent): `parted /dev/sdbX` 
- `sudo fdisk dev/sdbX` - another alternative
- after creating a partition you need to create a file system: `sudo mkfs -t ext4 dev/sdbX` 

### Instalation/Uninstallation
- source vs binary install - for source installation you need to build it yourself so you have more freedom, for binary installation - less freedom more easily installed. 
- when installing from source put the source code in `/usr/local/src` 
- put the standalone packages in `home/opt`
- put the standalone packages in _/opt_ directory
- `sudo apt-get remove <app name>`
- `sudo apt-get purge <app name>` - remove with all dependencies 
- `sudo dpkg -i packagename.deb` to fetch a specific package
- `sudo dpkg --remove <package name>` to remove package
- `sudo dpkg --purge <package name>` to remove with all dependencies 
- `sudo ldconfig -v` - to update shared library cache
- `apt-cache search` - search for a specific installed/installable package
- `apt-cache policy <package>` - check the version of the installed package
- `sudo apt-get update` - to update package lists 
- `sudo apt-get upgrade` - to upgrade system level packages from _/etc/apt/source.list_. Run after _update_
- to "**rollback**" a package check `tail -n25 /var/log/apt/history.log` for the package that was upgraded, then go to _/var/cache/apt/archives_ and do  `sudo dpkg -i <package_name>`

- sources list: `/etc/apt/sources.list and /etc/apt/sources.list.d`
- for python packages use `pip install <package_name>`
---
- `usr/bin` - for system wide installations
- `usr/local/bin` - for local installations 
---

### Archiving/extraction
- to extract with tar: `tar -xf archive.tar -C [directory]`
- to create a tar archive from file/files: `tar -cf [archive-name].tar.gz <file1> <file2>`
- to create a tar archive from a directory: `tar -zcvf [archive-name].tar.gz <directory-name>`

- archive manager - file-roller, open with sudo priviledges: `sudo file-roller <filename>`

### Hardware interfacing
- Launch terminal session (tty): `CTRL+ALT+F2`
- check for USB connected devices `lsusb`
- check hardware messages for a specific device `dmesg |grep xbox`
- check hardware level changes happening real-time `watch "dmesg | tail -20"`

### Search

- `locate [filename]` - system wide search for a specific file. `locate -b '/NAME'` - to specifically search for file named NAME
- `find -name [filename]` to search for a filename in the current and below directories  
- `which [package_name/app_name]` - package installation folder 
- grep - powerful search tool `grep <searchphrase> *` - find [searchphrase] among all files in current __dir__ 
- also add `...|grep [search_name]` to any command with terminal output to show only specific data

### Networking 

- check IP address:`hostname -I`
- check wlan config: `iwconfig`
- check wifi signal quality: `iwlist scan | egrep -i 'ssid|level'`
- select wifi antenna: `sudo modprobe -r [rtl8821ae] & sudo modprobe [rtl8821ae] ant_sel=[option]`
- `netstat -s | egrep -i 'loss|retran'` check TCP packet loss

- check wlan adapter properties `iw list`
- check firewall status: `sudo ufw status`

- open connection editor in terminal with `nm-connection-editor`
- open connection settings file `sudo gedit /etc/NetworkManager/system-connections/[connection name]`

- `nmap -v -sn 192.168.x.x_start-x_finish` to scan open ports
- `nc -v -w1 [IP_addr] [port]` - to check port connectivity 
- `netcat -z -v [IP_addr] [1-1000]` to **scan for ports**. Add `-u` to check for UDP ports 
- `lsof -i` to check used ports
- also `arp -a`
- `ss -t/-u/-x` view tcp udp or unix connections
- `sudo systemctl restart network-manager` or `sudo service network-manager restart ` - if the network hangs and after doing changes

- connecting through ssh: `ssh user@host` _host=hostname or IP address_. Add option -X for x-session
- copying a file through _ssh_ from A to B while logged in to A: `scp [dir/on/A/filename] user@host_B:[~/destination/dir/on/B]`
- copying a file through _ssh_ from A to B while logged in to B: `scp user@host_A:[dir/on/A/filename] [~/destination/dir/on/B]`  
- mounting remote directory to local directory: 
```
sudo sshfs -o allow_other user@host:[/path/to/remote/dir] [/path/to/local/dir]
```
- **First** add the user to the remote machine with `sudo adduser [username]`
- you also have to add the [user] as the owner of the remote directory using `sudo chown -R user:group [/path/to/remote/dir]` on the remote machine. 
- also uncomment the user_allow_other line in _/etc/fuse.conf_ on the local machine to allow the option allow_other
- unmounting: `sudo umount [/path/to/local/dir] ` 

- if ssh complains about keys do `ssh-keygen -R [ip_address]` to update known_hosts. You can also disable ssh key check (not recommended)

### Editing files

- `sed 's/hello/bye' input.txt > output.txt` - replace all the instances of _hello_ in __input.txt__ to _bye_ and print it out in __output.txt__
- `sed -i 's/hello/bye' file.txt` - does the same, but directly in __file.txt__ without creating output. 
- difference between two files `diff <file1> <file2>`

### Fonts 

- search for a filename that matches a specific font `fc-match Arial`
- fonts are stored in _/usr/share/fonts/_ and libre office fonts in _/usr/share/fonts/truetype_

### Troubleshooting
- for bootloader related issues (Windows option disappearing etc) do `sudo update-grub` or download **boot-repair**
- check running processes: ` ps aux | grep <process name>`
- check **system logs** with `journalctl` do `journalctl -b -1 -e` to view the end of last boot
- 


### GIT

- merge changes from master branch into feature branch: `git checktout [feature branch] & git merge master`
- `grip [FILENAME].md` for converting markdown to pdf.  Click on the generated link (http://localhost:6419/) in the terminal to open up the document in the browser, then print the browser window.
___

## Raspbian/DietPi

### General
- *htop* for system monitor 
- RT_PREEMT on Pi for soft real time 
- install gcc 6 on Jessie
- check version:  *gcc -v*
- configure network in `/etc/network/interfaces` or `/etc/dhcpcd.conf`
- check if there is internet `ping -c3 google.com`
- Set the clock before building, otherwise you gonna get "clock scews" `sudo date -s <YYYY-MM-DD HH:MM>`
### Static IP
- open `sudo nano /etc/network/interfaces`
- change to: 
```
iface wlan0 inet static
address [desired IP]
netmask [your network netmask]
gateway [your network gateway]
``` 
- disable DHCP client daemon and switch to standard networking:
```
sudo systemctl disable dhcpcd
sudo systemctl enable networking
``` 

### Serial ports
- enable serial port (uart0) in `/boot/config.txt`
- user has to be part of _dialout_ group!
- view serial output with *screen*: `sudo screen /dev/{serial_port} {baud_rate}`
- detach screen: `CTRL-A+D`
- closing a serial session: 
 1. check for session port `screen -ls`
 2. close: `screen -r [port] -X kill`
 
 to check which dev/tty port your specific device:
 
 1. do `ls /dev/ > devlist1.txt` before plugging in the device
 2. plug in the device
 3. do `ls /dev/ > devlist2.txt`
 4. `diff devlist1.txt devlist2.txt`
___

## Compiling and debugging with cpp

### gcc/g++

g++ and gcc can do the same thing, the only difference is the libraries they link against by default:
- g++  *is equivalent to:* `gcc -xc++ -lstdc++ -shared-libgcc`. __g++__ is pre-set for cpp compiling. 

- *g++ source1.cpp source2.cpp source3.cpp... -ldl -o executable_name -options*
- *options could be: -std=c++11; ....*
- linking against .so with g++:
1. set LD library path in .bashrc: `export LD_LIBRARY_PATH=/home/marius/Documents/ACROBAT/VICON/Linux_DataStreamSDK1_5`
2. source .bashrc
3. `g++ -o test_2 test_2.cpp -L [/library_direcotry]`

- fo debugging install clang++ `sudo apt get install clang`
- add [tasks.json](/docs/clang/tasks.json) and [launch.json](/docs/clang/launch.json) to your favourite IDE
- set a breakpoint


- For threading need to use *-pthread* option 
___

### Make

Make is a build-system that builds the code so that you wouldn't have to recompile everything by hand when you make changes. Also _Geany_ does not compile projects (more than one source file)! Make uses __Makefiles__ to describe the build processes:

```make
# final executable
all: Force_control_test_1

# object files
OBJ = Force_control_test_1.o read_param.o read_traj.o Trajectory_control.o
# Header files:
HDRS = Trajectory_control.h

# express dependencies  
$(OBJ) : $(DEPS)

# compile
Force_control_test_1: $(OBJ)
	$(CXX) -o $@ $^ -ldl -std=c++11

#cleanup:
clean:
   rm -f $(OBJ) Force_control_test_1


```
- *$@ - target to be built* 
- *$^ - list of dependencies*

- `make` in the folder of the Makefile to build an executable
- `make install` do a system user installation 
- `make deb` to build a .deb package
___

### CMake

CMake is a generator for build-systems. Executable builder/wrapper that generates all that is needed for building apps including the __Makefile__. Usage:

`cmake [options] <path-to-source>` - make sure to set _path to source_ were _CMakeLists.txt_ file is! this is usually one directory below `../`


### Cross-compiling

Code compiling for different target platform is supported by:

- CMake
- Qt

## .sh scripts

Making .sh scripts executable:
1. add **#!bin/bash** to the top of the sh script
2. do `chmod +x <script>.sh`
3. `export PATH=$PATH:/appropriate/directory`
4. appropriate directory storing sripts would be `$home/bin` 

___


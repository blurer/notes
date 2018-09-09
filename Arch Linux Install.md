# Arch Linux Install Guide

* Pre-Installation
  * set time with `timedatectl set-ntp true`
  * Partition disk
    * Identify what disk you're installing to with `fdisk -l` or `lsblk`
    * The disk I'm installing to is /dev/sdc - replace this with your disk 
    * we will need /boot, /, /home, and swap
    * drop into fdisk promtp `fdisk /dev/sdc`
    * print partitions with `p`
    * delete current partitions with `d`
    * after deleting current partitions, write changes with `w`
    * create a new partiton with `n` - we're going to create out boot partiton first
    * press enter for the default partition number
    * press enter for default first sector
    * for the last sector, you will select how large you want the partition to be. I'm choosing `+550M` here since I'm using an EFI system
    * If you're prompted by a message station that the partition contains a vfat signature, press `Y` to remove it.
    * Press `n` again to create a new partition, pressing enter through the default partition number and first sector size
    * This will be the swap partition. I don't really know how useful this is since I have a lot of RAM and never sleep the computer. I'm entering `+8G` for the last sector size.
    * Next partition is root, follow the prompts again and select a size sufficient for everything you think you will need to install. I'm going with `60G`
    * Last partition will be /home, just press enter through the prompts to use the rest of the disk.
    * After that press `w` to write the changes to the disk. Drop back into the fdisk prompt and press `p` to check that they're all there.
    * Here is the layout that I'll be using:
      * /dev/sdc1 550M - /boot
      * /dev/sdc2 8G - swap
      * /dev/sdc3 60G - /
      * /dev/sdc4 397.2G - /home
    * Now we need to set the partition type for our EFI boot partition. Press `l` to list partition types, we'll be using 1 EFI System. Press `t` to change partition types, select the partition, 1 in my case and press 1 to change it to EFI System. Again, press `w` to write these changes.

* Creating Filesystems
  * After partitioning the disks you will need to create the filesystems on those partitions. You will use the `mkfs` command for that. 
  * Here are the commands I used to make the filesystems:
    * `mkfs.vfat /dev/sdc1` - EFI Boot has to be vfat
    * `mkswap /dev/sdc2` - sets swap
    * `swapon /dev/sdc2` - enables swap
    * `mkfs.ext4 /dev/sdc3` - ext4 on /
    * `mkfs.ext4 /dev/sdc4` - ext4 on /home

* Mounting Filesystems
  * Mount your root file system at /mnt
    * `mount /dev/sdc3 /mnt`
  * Make directories to mount /boot and /home
    * `mkdir /mnt/home`
    * `mkdir /mnt/boot`
  * All of your file systems are now mounted

* Installation
  * Install is super easy, just issue the command below
    * `pacstrap /mnt base base-devel` - this will install the base package group and the base-devel package group

* Post Install
  * First thing is to configure fstab so your partitions are mounted at boot
    * `genfstab -U /mnt >> /mnt/etc/fstab` -U mounts by UUID, you can do drive labels with -L if you like.
    * `cat /mnt/etc/fstab` - verify all of your partitions are there and correct
  * Change root into the installation
    * `arch-chroot /mnt`
  * Set timezone
    * `ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime` - This will set the time for US EST
  * `hwclock --systohc` - generates /etc/localtime
  * Check that the time is correct with `date`
  * Uncomment en_US.UTF-8 UTF-8 and other needed locales in /etc/locale.gen
    * `vi /etc/locale.gen`
  * Generate locale with `locale-gen`
  * Set the LANG variable in /etc/locale.conf
    * `vi /etc/locale.conf` - this will be a new file
    * add "LANG=en_US.UTF-8" without the quotes on the first line
  * Set the hostname in /etc/hostname
    * `vi /etc/hostname`
  * Edit info in hosts
    * `vi /etc/hosts`
    * Mine looks like this:
      ```
       127.0.0.1  localhost
       ::1        localhost
       127.0.0.1  rt-arch.rtynerlabs.io rt-arch
      ```
  * Set root password
    * `passwd`
  * Install and enable a network manager to get DHCP
    * `pacman -S networkmanager` - there are others, but I use NetworkManager.
    * `systemctl enable NetworkManager`
  * Install and configure GRUB
    * `pacman -S grub`
    * `pacman -S efibootmgr`
    * `grub-install --target=x86_64-efi --efi-directory=/mnt --bootloader-id=GRUB`
    * `grub-mkconfig -o /boot/grub/grub.cfg` - creates GRUB config
  * Unmount your drives and reboot
    * `exit` - Exits chroot environment
    * `umount -R /mnt`
    * `reboot now`
  
* Post Reboot
  * Create your user and group, and grant sudo permissions
    * `groupadd rt`
    * `useradd -m -g rt -s /bin/bash rt`
    * `passwd rt`
    * edit sudoers file
      * `visudo`
        ```
        ## User privilege specification
        ##
        root  ALL=(ALL) ALL
        rt    ALL=(ALL) ALL
        ```
    * `logout` - don't `su`, it breaks `startx` in the next section
    * Login with your new user
  
  * * Install graphical environment
    * `sudo pacman -S xorg-server xorg-xinit xorg-xrandr`
    * `sudo pacman -S i3 dmenu` - window manager
    * edit ~/.xinirc to start i3
      ```
      exec i3
      ```
    * `sudo pacman -S rxvt-unicode` - terminal emulator
    * `sudo pacman -S ttf-hack` - terminal font
    * `sudo pacman -S ttf-dejavu ttf-liberation` - fixes firefox fonts
    * `sudo pacman -S nvidia` - graphics drivers
    * `sudo pacman -S pulseaudio` - if you want sound
    * `reboot` - reboot is needed after graphics drivers installation
    * After reboot, login at the tty and type `startx`
    * Now you are ready to configure your window manager

* Install Packages & Other Configuration
  * After you're in the graphical environment, you're ready to install anything else you'll need. Some of my go to's are below.
  * `sudo pacman -S firefox chromium zsh unzip git htop python python-pip vim wget feh compton rofi tmux ranger`
  * yay - AUR helper, allows for software installation from Arch User Repo
    * `git clone https://aur.archlinux.org/yay.git`
    * `cd yay`
    * `makepkg -si`
  * `yay polybar`
  * `yay discord-canary`
  * Create or copy over ssh keys
    * create new
      * `ssh-keygen -t rsa`
    * copy ssh keys
      * `mkdir ~/.ssh`
      * `cp $PATH_TO_SSH_KEYS$ ~/.ssh`
      * `chmod 700 ~/.ssh`
      * `chmod 644 ~/.ssh/id_rsa.pub`
      * `chmod 600 ~/.ssh/id_rsa`
  * Create directories
	* `mkdir ~/working`
	* `mkdir ~/projects && cd ~/projects && git init`
	* `git config --global user.email "EMAIL"`
	* `git config --global user.name "NAME"`
  * Clone repos
	* `cd ~/working && wget https://raw.githubusercontent.com/rtyner/misc-scripts/master/git-clone.sh && chmod u+x git-clone.sh && ./git-clone.sh`
  * Symlink dotfiles
	* `ln -s /home/rt/projects/dotfiles/zsh/.zshrc /home/rt/.zshrc`
	* `ln -s /home/rt/projects/dotfiles/vim/.vimrc /home/rt/.vimrc`
	* `ln -s /home/rt/projects/dotfiles/i3/config /home/rt/.config/i3/config`
	* `ln -s /home/rt/projects/dotfiles/polybar/config /home/rt/.config/polybar/config`
	* `ln -s /home/rt/projects/dotfiles/rofi/config /home/rt/.config/rofi/config`
	* `ln -s /home/rt/projects/dotfiles/xresources/.Xresources /home/rt/.Xresources`
	* `ln -s /home/rt/projects/dotfiles/xresources/.xinitrc /home/rt/.initrc`
  * Setup Vim plugins
	* `mkdir -p ~/.vim/autoload ~/.vim/bundle && curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim`
	* `cd ~/.vim/bundle && git clone git://github.com/arcticicestudio/nord-vim.git`
	* `cd ~/.vim/bundle && git clone https://github.com/tpope/vim-fugitive.git && vim -u NONE -c "helptags vim-fugitive/doc" -c q`
	* `git clone https://github.com/itchyny/lightline.vim ~/.vim/bundle/lightline.vim`




















ARCH LINUX INSTALL CHEATSHEET
=============================
### VM installation
-------------------
#### PRE-INSTALL
-------------------

1. **check internet connectivity**
    ```bash
    ping google.com
    ```
2. **list your storage devices**
    ```bash
    lsblk #Visually easier
    fdisk -l #Contains a lot of text
    ```
3. **create your partitions**
    ```bash
    cfdisk #select GPT for UEFI support (if you prefer, investigate about MBR and GPT)
    ```
    Create one partition *swap*, another to mount the *root* directory and another to mount the EFI system
    

    GPT partitions:
    

    ![alt text](C:\Users\alexa\OneDrive\Pictures\Screenshots\ARCH1.png)
4. **format partitions with their file system**
    ```bash
    mkfs.ext4 /dev/*root_partition* #this will set your file system to be ext4
    mkswap /dev/*swap partition*
    mkfs.fat -F 32 /dev/efi_system_partition 
    ```
5. **mount file systems**
    ```bash
    mount /dev/*root_partiton* /mnt 
    mount --mkdir /dev/*efif_system* /mnt/boot #mount the volume on a new created directory
    swapon /dev/*swap_partition* #enable swap volume
    ```


------------
#### INSTALL
------------

6. **Install packages to run Arch**
    ```bash
    pacstrap -K /mnt base linux linux-firmware
    ```
    This way we'll install the necessary packages to run the root system that's been mounted

    
    We are installing this from cache with -K
7. **Generate the fstab file**
    ```bash
    genfstab -U /mnt >> /mnt/etc/fstab
    ```
8. **Chroot in the system**
    ```bash
    arch-chroot /mnt 
    ```
9. **Set time zone**
    ```bash
    ln -sf /usr/share/zoneinfo/America/Mexico_City /etc/localtime #creates a symbolic link that points to /usr...
    ```
10. **Generate locales**
    ```bash
    locale-gen
    nano /etc/locale.conf
    #set variable LANG=en_US.UTF-8 or choose the right one
    ```
11. **Network configuration**
    ```bash
    /etc/hostname #set your device's hostname 
    ```
12. **Set the root password**
    ```bash
    passwd
    ```
13. **Install the grub boot loader**
    ```bash
    mount /dev/efi_system /boot/efi #will mount the efi system partition to the /boot/efi
    pacman -S grub efibootmgr
    grub-install --target=x86_64-efi --efi-directory=/boot/efi 
    grub-mkconfig -o /boot/grub/grub.cfg
    ```


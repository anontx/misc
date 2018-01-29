# Arch Linux安装记录

*UEFI*, *x86_64*

## 1. 创建硬盘分区  

  使用 **parted** 工具创建，分区方案如下表：

| Mount point | Partition | Type | Bootable Flag | Size |  
| - | - | - | :-: | - |  
| /boot | /dev/sda1 | EFI System Partition | Yes | 512M |
| / | /dev/sda2 | ext4 | No | Remainder of the device |


    mklabel gpt
    
    # /boot
    mkpart ESP fat32 1MiB 513MiB
    set 1 boot on

    # /
    mkpart primary ext4 513MiB 100%


## 2. 格式化分区

    mkfs.vfat -F32 /dev/sda1
    mkfs.ext4 /dev/sda2


## 3. 挂载分区

    mount /dev/sda2 /mnt
    mount /dev/sda1 /mnt/boot


## 4. 修改镜像

    vim /etc/pacman.d/mirrorlist


## 5. 安装基本系统

    pacstrap /mnt base base-devel


## 6. 生成fstab

    genfstab -U /mnt >> /mnt/etc/fstab


## 7. chroot

    arch-chroot /mnt


## 8. 设置时区

    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    hwclock --systohc --utc


## 9. 设置区域

    vim /etc/locale.gen
    # en_US.UTF-8 UTF-8

    locale-gen
    echo LANG=en_US.UTF-8 > /etc/locale.conf


## 10. 修改主机名

    echo master > /etc/hostname

    vim /etc/hosts
    # 127.0.0.1 localhost.localdomain	localhost
    # ::1       localhost.localdomain	localhost
    # 127.0.1.1 master.localdomain      master


## 11. 修改密码

    passwd


## 12. 安装boot loader

    pacman -S grub
    pacman -S efibootmgr

    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
    grub-mkconfig -o /boot/grub/grub.cfg


## 13. 重启

    # 退出chroot环境
    exit

    # 卸载挂载分区
    umount -R /mnt
    
    reboot

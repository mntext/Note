## time sync
```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

## grub
```
grub-install --target=x86_64-efi --bootloader-id=arch --boot-directory=/boot
grub-mkconfig -o /boot/grub/grub.cfg
```

## windows manager
```.xinitrc
exec awesome
```

## bigger windows
```.Xresources
Xft.dpi:        156
Xft.antialias:  true
Xft.hinting:    true
Xft.rgba:       rgb
Xft.hintstyle:  hintslight
```
## Fonts
* Chinese:wqy-zenhei
* English:ttf-

## Sound
```.asoundrc
defaults.pcm.card 1
defaults.pcm.device 0
defaults.ctl.card 1
```

## input-method
```.pam_environment
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

## touchpad-libinput
```/etc/X11/xorg.conf.d/30-touchpad.conf
Section "InputClass"
    Identifier "pointer"
    Driver "libinput"
    Option "Tapping" "on"
    Option "ClickMethod" "clickfinger"
EndSection
```

## xp key
MRX3F-47B9T-2487J-KWKMF-RPWBY

# 在windows下操作EFI分区
```
diskpart #输入diskpart进行磁盘管理。
输入list disk 查看电脑上挂载的硬盘。
输入select disk 0 选择编号为0的硬盘进行操作。
输入list vol 查看我们的分区表。
输入select vol 6选择要操作的磁盘号
输入ass让系统给他分配一个驱动号或装载点。
```

https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/



# GnuPG-2.1 与 pacman 密钥环
2014 年 12 月 8 日

由于升级到了 gnupg-2.1，pacman 上游更新了密钥环的格式，这使得本地的主密钥无法签署其它密钥。这不会出问题，除非你想自定义 pacman 密钥环。不过，我们推荐所有用户都生成一个新的密钥环以解决潜在问题。

此外，我们建议您安装 haveged，这是一个用来生成系统熵值的守护进程，它能加快加密软件（如 gnupg，包括生成新的密钥环）关键操作的速度。

要完成这些操作，请以 root 权限运行：
```
pacman -Syu haveged
systemctl start haveged
systemctl enable haveged

rm -fr /etc/pacman.d/gnupg
pacman-key --init
pacman-key --populate archlinux
pacman-key --populate archlinuxcn
```

## Arch Linux CN 源

简介:Arch Linux 中文社区仓库是由 Arch Linux 中文社区驱动的非官方用户仓库。包含中文用户常用软件、工具、字体/美化包等。

仓库地址：http://repo.archlinuxcn.org
使用说明

在 /etc/pacman.conf 文件末尾添加两行：

[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

然后请安装 archlinuxcn-keyring 包以导入 GPG key。








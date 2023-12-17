---
date: 2023-12-15T18:17:28+08:00
title: "配置Eos Linux"
draft: false
url: "/learn/2-1"
keywords:
- EosLinux
categories:
- 学
tags:
- hugo博客
---

## 基本

```bash
systemctl stop reflector.service
systemctl disable reflector.service
eos-update
sudo pacman -S yay
yay -Syyu
sudo pacman -S paru
sudo pacman -S pacman-contrib
sudo pacman -S discover
sudo pacman -S filelight
yay -S marktext-bin
yay -S octopi
yay -S brave-bin
yay -S keepassxc
sudo pacman -S libreoffice-still libreoffice-still-zh-cn
sudo pacman -S firefox-developer-edition
sudo pacman -S vlc
sudo pacman -S thunderbird thunderbird-i18n-zh-cn
sudo pacman -S obs-studio
yay -S schildichat-desktop-bin
sudo pacman -S tabby-bin
paru -S vscodium-bin
```

# 安装fcitx5-rime输入法

```bash
sudo pacman -S fcitx5-im
sudo pacman -S fcitx5-material-color
sudo pacman -S fcitx5-rime
yay -S ruby
```

这里直接使用自动部署脚本。

```bash
git clone --depth=1 https://github.com/Mark24Code/rime-auto-deploy.git --branch latest
cd rime-auto-deploy
./installer.rb
```

![](https://img.0pt.icu/learn/init-eos-linux/1.png)

输入3回车。

![](https://img.0pt.icu/learn/init-eos-linux/2.png)

输入1回车。

然后在`/etc/environment` 文件末尾添加：

```/etc/environment
export GTK_IM_MODULE=fcitx5

export QT_IM_MODULE=fcitx5

export XMODIFIERS=@im=fcitx5
```

保存后退出，重启电脑之后，输入法就可以用了。

# 安装NVIDIA显卡驱动

```bash
yay -S nvidia-inst
nvidia-inst --drivers
```

![](https://img.0pt.icu/learn/init-eos-linux/3.png)

需要等一会，不要着急。

![](https://img.0pt.icu/learn/init-eos-linux/4.png)

这里我的`series`是`545`

```bash
nvidia-inst --series 545 -t
```

将`--series`参数后面的`545`改为你自己的`series`

例如以`390`为例，就是

```bash
 nvidia-inst --series 390 -t
```

![](https://img.0pt.icu/learn/init-eos-linux/5.png)

将其中的这个：

```bash
COMMANDS TO RUN:
    pacman -Rs --noconfirm --noprogressbar --nodeps nvidia-dkms nvidia-hook nvidia-utils
    yay -Syu nvidia-390xx-dkms nvidia-390xx-utils nvidia-390xx-settings
```

里面的COMMANDS TO RUN给出的命令执行就可以安装显卡驱动。

```basg
pacman -Rs --noconfirm --noprogressbar --nodeps nvidia-dkms nvidia-hook nvidia-utils
yay -Syu nvidia-390xx-dkms nvidia-390xx-utils nvidia-390xx-settings
```

等待安装。

安装后执行：

```bash
nvidia-smi
```

来查看显卡驱动有没有正确安装。执行后有如下输出则证明显卡驱动安装成功。

![](https://img.0pt.icu/learn/init-eos-linux/6.png)

# 配置btrfs

```bash
yay -S snapper
yay -S snap-pac
yay -S grub-btrfs
yay -S btrfs-assistant
yay -S btrfsmaintenance
yay -S snap-pac-grub
```

然后，请先打开一下 Btrfs Assistant 这个应用，退出后再运行下面这个：

```bash
yay -S snapper-support
```

# vscodium更换官方插件源

找到`vscodium`的安装的文件夹。这里，我的eos的路径是`/opt/vscodium-bin`

在`/opt/vscodium-bin/resources/app` 中找到`product.json` 这个文件。

打开该文件，找到如下内容：

```product.json
  "extensionsGallery": {
    "serviceUrl": "https://open-vsx.org/vscode/gallery",
    "itemUrl": "https://open-vsx.org/vscode/item"
  },
```

将其改为：

```product.json
    "extensionsGallery": {
        "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
        "itemUrl": "https://marketplace.visualstudio.com/items"
    },
```

保存后重启vscodium，更换完毕。

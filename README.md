# ![OpenWRT](images/openwrt_logo.png) firmware autobuilder 

Original from [P3TERX](https://github.com/P3TERX/Actions-OpenWrt)  
Further modded  by [Eliminater74](https://github.com/DevOpenWRT-Router/Action_OpenWRT_AutoBuild_Linksys_Devices)

#### Made to simplify the firmware compilation thanks to the GitHub actions.
##### Ready to build firmware for Linksys wrt32x & a8450 (aka. Belkin rt3200)
<p align="middle">
  <img width="300" height="auto" src="images/wrt32x.jpg" />
  <img width="300" height="auto" src="images/e8450_side.png" />
</p>

Linksys **wrt32x**:

```
Target System: "Marvell EBU Armada"
Subtarget: "Marvell Armada 37x / 37x / XP"
Target Profile: "Linksys Venom (Linksys WRT32X)" and "Linksys WRT32X"
```

Linksys **a8450**:

```
Target System: "MediaTek Ralink ARM"
Subtarget: "MT7622"
Target Profile: "Belkin RT3200 UBI" and "Linksys E8450 UBI"
```

> Snapshot changelog: https://git.openwrt.org/?p=openwrt/openwrt.git;a=shortlog

## Features:
Only for wrt32x:
- Patches taken from: [Divested-WRT](https://divested.dev/unofficial-openwrt-builds/mvebu-linksys/patches/)
- mwlwifi driver taken from: [Lean's OpenWRT](https://github.com/coolsnowwolf/lede/tree/master/package/kernel/mwlwifi)
- The DFS channels don't work, despite leaving the region code as it comes by default... So BT was removed (*kmod-mwifiex-sdio, mwifiex-sdio-firmware, kmod- bluetooth, kmod-btmrvl, kmod-mmc*) to see if this way the DFS channels work

Common:
- **NetData SQM char** from: https://github.com/Fail-Safe/netdata-chart-sqm ([how to set up](https://github.com/ferboiar/wrt32x/wiki/Build-configuration-tips#netdata-sqm-char "how to set up")) 
- **OpenWRTScripts** from: https://github.com/richb-hanover/OpenWrtScripts
- **autoSQM script** from: https://github.com/baguswahyu/autoSQM-damasus.bagus ([how to set up](https://github.com/ferboiar/wrt32x/wiki/Build-configuration-tips#autosqm_script "how to set up"))
- More scripts :
  - /usr/local/bin/opkg_list_installed.sh: list the user installed packages (https://gist.github.com/benok/10eec2efbe09070150ed2100d29dc743)
- **Network interfaces ports status** from: https://github.com/tano-systems/luci-app-tn-netports ([how to set up](https://github.com/ferboiar/wrt32x/wiki/Build-configuration-tips#network_port_status "how to set up")) 
- Especific '**Cryptographic Hardware Accelerators**' set up (https://openwrt.org/docs/techref/hardware/cryptographic.hardware.accelerators). Some more detail [here](https://github.com/ferboiar/wrt32x/wiki/Cryptographic-Hardware-Accelerators "here").
- **Wireguard** (*wireguard-tools, luci-proto-wireguard, luci-app-wireguard, kmod-wireguard*)
- **OpenVPN** server/client (*openvpn-openssl, openvpn-easy-rsa, luci-app-openvpn, kmod-tun*)
- **USB Storage** (*kmod-usb-storage, kmod-usb-storage-extras, kmod-usb-storage-uas, kmod-usb-ohci, kmod-usb-uhci, kmod-usb2, kmod-usb3, kmod-fs-ext4, kmod-fs-vfat, kmod-fs-ntfs, kmod-scsi-core, kmod-nls-cp437, kmod-nls-iso8859-1, block-mount, block-hotplug, e2fsprogs, usbutils, usbids, ntfs-3g*)
- **NetData** (*netdata, bash, coreutils-timeout, curl*). Access through http://router_ip:19999. luci-app-netdata doesn't work with firefox at least
- **Atheros 9k WIFI driver** (*ath9k-htc-firmware, kmod-ath, kmod-ath9k-common, kmod-ath9k-htc*)

_______________________________________________________________________
![GitHub Downloads](https://img.shields.io/github/release-date/ferboiar/wrt32x?style=flat-square&logo=openwrt) 

![GitHub Downloads](https://img.shields.io/github/downloads/ferboiar/wrt32x/total?style=for-the-badge&logo=openwrt) 
![GitHub Downloads](https://img.shields.io/github/downloads/ferboiar/wrt32x/latest/total?style=for-the-badge&logo=openwrt) 


### Actions-OpenWrt
[![Visits Badge](https://badges.pufler.dev/visits/ferboiar/wrt32x)](https://badges.pufler.dev) 
[![Cleaning](https://github.com/ferboiar/wrt32x/actions/workflows/cleanup.yml/badge.svg)](https://github.com/ferboiar/wrt32x/actions/workflows/cleanup.yml) 
[![Build wrt32x firmware ](https://github.com/ferboiar/wrt32x/actions/workflows/build-wrt32x.yml/badge.svg)](https://github.com/ferboiar/wrt32x/actions/workflows/build-wrt32x.yml)

### Repo Updated:
[![Updated Badge](https://badges.pufler.dev/updated/ferboiar/wrt32x)](https://badges.pufler.dev) 

![Your Repository’s Stats](https://github-readme-stats.vercel.app/api?username=ferboiar&show_icons=true)

_______________________________________________________________________
## INSTRUCTIONS:
1. Click the `Use this template` button from my repo to create your new repo (or fork it)
2. in the home folder of your linux machine clone the openwrt github repo and yours:
```
git clone https://github.com/openwrt/openwrt.git
git clone https://github.com/ferboiar/wrt32x.git *change it for your brand new recently created repo 
```
3. Copy in the new folder "openwrt/" the scripts and the configuration files from your repo:
```
cp wrt32x/scripts/*.sh openwrt/
chmod +x openwrt/*.sh
mkdir openwrt/patches/
cp -r wrt32x/configs/patches/ openwrt/
cp wrt32x/configs/feeds.conf.default openwrt/
cp wrt32x/configs/wrt32x.config openwrt/
```
4. run, from "openwrt/" the commands contained in "manual_generate.sh" script:
```
./fetch_packages.sh
./scripts/feeds update -a
./scripts/feeds install -a
./scripts/feeds uninstall bluld
cp wrt32x.config .config
git am patches/*.patch
./functions.sh BUILD_USER_DOMAIN
./functions.sh PRE_DEFCONFIG_ADDONS
sudo ./functions.sh CCACHE_SETUP
./functions.sh DEFAULT_THEME_CHANGE
```
5. then `make menuconfig`, load your .config file and choose the packages you want.
6. upload your .config file to your repo "/configs" as wrt32x.config
7. launch "**Actions**" > "**Build wrt32x firmware (Linksys Device)**"> "**Run workflow**" on YOUR repo

after 4 or 5h of compilation you will see new files into "Artifacs".

### Flashing process
If you are not sure about your kernel partition size do a sysupgrade from command line [Increasing mamba and venom kernel partition to 6MB](https://forum.openwrt.org/t/increasing-mamba-and-venom-kernel-partition-to-6mb)

1. Verify compatibility
```
   $ fw_printenv | grep "pri_kern_size"; #mamba MUST equal 0x400000
   $ fw_printenv | grep "priKernSize"; #venom MUST equal 0x0600000
```
Do not try to change them!

2. Flashing process:
You must always use a factory image when flashing to or away from a resized build.
- Create a backup tar
- Flash the image via sysupgrade: `sysupgrade -F squashfs-factory.img`
- Restore the backup

Two times, to do the change from both boot partitions. Once, you will be capable of install updates without do this again

_______________________________________________________________________

### TIPS
- Note that your repo needs to be public, otherwise you have a strict monthly limit on how many minutes you can use.
- Your session can run for up to six hours. Don't forget to close it after finishing your work, otherwise you will continue to occupy this virtual machine, making it impossible for others to use it normally.
- Please check the [GitHub Actions Terms of Service](https://docs.github.com/en/github/site-policy/github-additional-product-terms#5-actions-and-packages). According to the TOS the repo that contains these files needs to be the same one where you're developing the project that you're using it for, and specifically that you are using it for the "_production, testing, deployment, or publication of [that] software project_".

Build OpenWrt using GitHub Actions: [Instructions (Use translator)](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

## Advanced
### SSH by using [ngrok](https://ngrok.com/)
Click the `Settings` tab on your own repository, and then click the `Secrets` button to add the following encrypted environment variables:

- `NGROK_TOKEN`: Sign up on the <https://ngrok.com> , find this token [here](https://dashboard.ngrok.com/auth/your-authtoken).
- `SSH_PASSWORD`: This password you will use when authorizing via SSH.

### Send connection info to [Telegram](https://telegram.org/)
Click the `Settings` tab on your own repository, and then click the `Secrets` button to add the following encrypted environment variables:

- `TELEGRAM_BOT_TOKEN`: Get your bot token by talking to [@BotFather](https://t.me/botfather).
- `TELEGRAM_CHAT_ID`: Get your chat ID by talking to [@GetMyID_bot](https://t.me/getmyid_bot) or other similar bots.

You can find Telegram Bot related documents [here](https://core.telegram.org/bots).

[![LICENSE](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square&label=License)](https://github.com/ferboiar/wrt32x/blob/master/LICENSE) ![GitHub Stars](https://img.shields.io/github/stars/ferboiar/wrt32x.svg?style=flat-square&label=Stars&logo=github) ![GitHub Forks](https://img.shields.io/github/forks/ferboiar/wrt32x.svg?style=flat-square&label=Forks&logo=github)

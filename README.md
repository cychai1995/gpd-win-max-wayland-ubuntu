# gpd-win-max-wayland-ubuntu
The following steps only tested on Ubuntu 20.04 with GPD Win Max.
# Steps
## 1. Generate EDID information
```
git clone https://github.com/akatrevorjay/edid-generator.git
# Install the required packages
sudo apt install zsh edid-decode automake dos2unix
cd edid-generator
./modeline2edid - <<< 'Modeline "800x1280" 84.99  800 856 944 1088 1280 281 1284 1302 -HSync +Vsync ratio=16:10'
make
```
The file __800x1280.bin__ shoud be found in the same folder.
Check the edid information
```
edid-decode 800x1280.bin
```
Make sure the __Detailed mode__ part is the same as your modeline setting.
Then copy the file to /usr/lib/firmware/edid
```
sudo mkdir /usr/lib/firmware/edid
sudo cp 800x1280.bin /usr/lib/firmware/edid
```

## 2. Update Grub
```
sudo vim /etc/default/grub
```
Edit the GRUB_CMDLINE_LINUX_DEFAULT, the framebuffer is also rotated.
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash drm_kms_helper.edid_firmware=edid/800x1280.bin fbcon=rotate:1"
```
Save and run `sudo update-grub`. Reboot and login to the Ubuntu on Wayland

## Reference links
1. [Wayland how to set a custom resolution](https://askubuntu.com/questions/973499/wayland-how-to-set-a-custom-resolution)
2. [Manually add a resolution to Gnome with Wayland](https://superuser.com/questions/1137574/manually-add-a-resolution-to-gnome-with-wayland)
3. [Arch-Kernel mode setting](https://wiki.archlinux.org/index.php/Kernel_mode_setting)




# Arch Linux installation guide on MacBook Pro 12,1
This is esentially for my own use, as I expect I will need to install this again in the future, but as it might be helpfull for others (I\'ve needed a lot of those guides too) I though to leave it accessible.\
I am no Linux expert, in fact I have only been using it for a few months, so I imagine some of the steps described might not be necessary.\
I used those steps to install it on a 13 inch MacBook Pro from early 2015 (the model is 12,1), but I would guess it is similar for other models.

## Installation

#### Requirements

* A macbook (I guess this goes without saying)
* A way to connect to Ethernet (That dongle life)
* A USB stick with the bootable Arch

#### Boot from USB

In order to start we need to boot from the USB stick with the Arch image, to do so insert the USB drive in the computer and reboot it pressing the `Alt` key.\
Once you can choose the boot device choose the USB stick, press `enter` and in the next menu press the letter `e`. Add `nomodeset ` to the beginning and continue with the launching. This will allow you to see the whole screen (without it the last lines in the console would not be visible). Once arch is loaded you can change the keyboard layout and the font (you will need a bigger font if you don\'t want to become blind with the retina display):

> loadkeys es\
> setfont sun12x22

This will set the keyboard layout to spanish and the font to a visible one.

#### Create and mount partitions

This is probably not necessary, but you can run `efivar -l` to display the EFI variables and make sure you will need to partition accordingly, so if nothing shows up after running that this guide is no bueno for you. Once that is out of the way let's partition using cgdisk. This is quite intuitive so just run `cgdisk /dev/sda` (of course change sda for whatever drive you want to use) and delete whatever is left there and create the partitions:

|id  |size|type|name|
|----|----|----|----|
|sda1|1G  |ef00|boot|
|sda2|100G|8300|root|
|sda3|12G |8200|swap|

Now you can run `lsblk` to make sure it is all good and continue with mounting the partitions:
* `mkfs.vfat -F32 /dev/sda1`
* `mkfs.ext4 /dev/sda2`
* `mkswap /dev/sda3`
* `swapon /dev/sda3`
* `mount /dev/sda2 /mnt`
* `mkdir -p /mnt/boot`
* `mount /dev/sda1 /mnt/boot`
* `genfstab -U -p /mnt >> /mnt/etc/fstab`

Now we will edit the file (`vim /mnt/etc/fstab`) to make sure the line of the ext4 partition ends with a "2", the line with the swap ends with a "0" and the line with the boot partition ends with a "1". This will configure partition checking on boot. Also ext4 partition options:
> rw,relatime,data=ordered,discard


## Links
Of course when I did this I checked multiple other guides, some of them are:

* [Arch Linux running on my MacBook](https://medium.com/@philpl/arch-linux-running-on-my-macbook-2ea525ebefe3
)
* [Installing Arch Linux on a 2017 Macbook Air](https://github.com/badgumby/arch-macbook-air)
* [Arch wiki Installation guide](https://wiki.archlinux.org/index.php/installation_guide)
* [Arch wiki Mac specific guides](https://wiki.archlinux.org/index.php/Mac)


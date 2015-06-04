---
title: Saving for Future Reference (Lenovo UEFI boot problem)
layout: post
---

I need to archive this post: http://ubuntuforums.org/showthread.php?t=2185869&p=12884470#post12884470 just so I don't have to
go looking for it in the future.  It was exactly what I needed for my specific problem getting Linux installed on my laptop.
This was his only post to the ubuntu forums, what a guy.  I owe him a beer.

In case the post is deleted for any reason, here's the text:

Just wanted to share this piece of information with anyone who has had troubles with efibootmgr because of faulty/crapy implementations from the good guys at Lenovo who probably coded the firmware using child labor in a mental asylum.

Hint: The UEFI thing looks for the default EFI/BOOT/BootX64.efi binary, regardless of what is in it's eprom so skip efibootmgr altogether; You pointlesly risk bricking your laptop.

```
mount /dev/sda1 /mnt
cd /mnt/efi
cp -rfv ubuntu boot
cd boot
mv grubx64.efi bootx64.efi
```

Edit boot order so it boots from the hard drive first.

Reboot and enjoy!

If you're using kubuntu and the installer failed, apt will nag you because it cannot finish configuring grub properly. The solution is:

```
sudo mv /usr/bin/efibootmgr /usr/bin/efibootmgr.lol #you must use .lol or else you run the risk of copyright infringement
sudo touch /usr/bin/efibootmgr
sudo chmod +x /usr/bin/efibootmgr
sudo apt-get install grub-efi #will pass and stop nagging
```

PS: If you use like me for instance a Lenovo Ideapad 205, suspend/shutdown will not work in EFI mode because of the poor quality of the rice the programmers were payed with. Additionally, the laptop won't wake up from suspend because of a bios bug. In this case, all you need to do is install a windows for a little while, upgrade your bios to the latest version (if it's below 21), then install ubuntu in bios mode.
The idea is that you can make a bootable usb using the nifty Startup Disk Creator, then mount it and rename the EFI folder to OLDEFI. It will then boot in bios mode and it will create a MBR partition table instead of GPT and it will install grub-bios or whatever it's alias is. Everything will work flawlessly, at least in 13.10

PS PS: Don't even bother with ati/amd/radeon drivers. Keep the default dri/glx thing. Frglx is pure crap and it's crash patterns are a better source of entropy then /dev/random. RSA should use ati drivers to regain it's customer's trust. 
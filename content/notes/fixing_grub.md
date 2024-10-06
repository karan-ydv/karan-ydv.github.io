---
title: "Fixing Grub"
date: 2024-05-12T12:00:52+05:30
tags: ["linux", "booting"]
description: "How I fixed efi partition and installed grub"
draft: true
---

Recently I upgraded a laptop to SSD from a hard disk. I got a 512GB ssd installed from a computer repair shop. They also installed a fresh copy of windows on the ssd and put the existing hdd to an external drive using a case. 

I was running manjaro linux that I had installed in a new partition. I couldn't afford to reinstall it on the ssd and configure it for days. I wanted the same os as it is on the ssd, so I needed to copy the root partition and update the efi partition on the ssd.

I used PartitionWizard to copy the partition (246 GB) to the ssd which was taking almost 3 hours when I had connected the external hdd to a usb2 port I cancelled it and restarted it by connecting it to the usb3 port which took a slightly longer than 1 hour. Now the efi partition had to be modified to load grub with boot options.

Tried to directly load manjaro using grub2 disk image but it didn't give manjaro option. I knew I would have to rebuild the grub using a live disk. Downloaded bodhi linux (1.3) gb using torrent with avg speed ~ 9 MB/S and transferred it to usb with speed 4 MB/S. 
I use ventoy to boot multiple images with a single usb drive.

![Ventoy](/images/ventoy.jpeg)

I tried to rebuild the grub using update-grub and grub-install but no luck. I gained understanding of some linux commands while doing this - chroot, mount, umount, lsblk, blkid, fdisk, used to handle partitions and drives.
I also learned about the contents of the efi partition - it is mounted on the location /boot/efi. Mainly it contains directories under a directory EFI - EFI/Boot, EFI/Manjaro, EFI/Microsoft. The Boot directory probably contains grub bootloader that appears on boot and the other OS directories contain bootx64.efi files which load the os.

Eventually I came across boot-repair which worked like a charm and did the job.
https://www.baeldung.com/linux/boot-repair-live-medium

Finally the partitions look like this
![Final Partitions](/images/final_partitions.png)


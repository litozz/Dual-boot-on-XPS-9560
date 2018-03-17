# Tutorial about how to install Linux distro (LD) dual boot on Dell XPS 9560

## 1. Configure first BIOS/UEFI options

First of all, enter de BIOS/UEFI setup. Set boot mode to UEFI and disable secure boot. Test if you can start windows, if yes, you can continue.

## 2. Allowing Windows to boot on AHCI mode


**By default**, Windows boots in Raid On mode and my LD (Ubuntu) boots on AHCI mode. For my case, Ubuntu is more important, so, I will configurate Windows to set it to boot on AHCI mode (you can skip this step if you don't mind reconfigurate BIOS each time you start windows, but it sucks, I recommend it).

- If you are logged in windows (currently SATA Raid On mode), open a `cmd` **as administrator** and type `bcdedit /set {current} safeboot minimal`. This allow you starts Windows by default on secure mode, even in SATA AHCI mode. Reboot on windows and, if Windows starts on SATA AHCI mode, you are in business.

- Now, open again `cmd` **as administrator** and type `bcdedit /deletevalue {current} safeboot`. This will remove the "Windows boot on secure mode by default" option you set in the previous step.

- Last step: reboot your computer and check if Windows starts with SATA AHCI mode in "normal" mode (not safe). If do, CHEERS!! this is an humongous success, believe it or not.

## 3. Installing your LD

Your SSD is on SATA AHCI mode and your LD can recognise it. Start with your live CD (a booteable pen-drive you can make with your LD ISO and rufus, Yumi or whatever). Once you've created it, insert the live CD and change your device boot order on BIOS/UEFI, setting USB at the first place. Your live cd should start. Choose install and your LD installation interface should show you an option that says something like "Install alongside Windows" (it depends on your LD), choose it. You can install it as you can/want.

In my case:
  * Primary partition: mount on ` /boot`, `EXT4` 16 GB (booting partition)
  * Extended partition: mount on `/`, `EXT4` logical 50GB
  * Extended partition: mount on `swap` at the beggining, 16GB [RAM GB if RAM >= 8GB, 2\*RAM if RAM < 8GB] `logical`
  * Extended partition: mount on `/home` `EXT4`, remaining GB in your LD partition, `logical`

At the finish of the installation, you should have your dual boot completely configured, so, the last step is enjoy your config. To test it, reboot twice: one for Windows and another for your LD.

## 4 Solving possible boot problems

For some reason some LD keeps frozen at the load screen when boots. For example if this happens in ubuntu, type `e` on GRUB menu, remove the `quiet splash ---` flag and add `nvme_load = YES nomodeset`. This should allow you to jump the load screen. This issue is common, for example, when you install new Nvidia drivers, if this happens, follow this process adding only `nomodeset` (it is not needed remove `quiet splash ---`).

If you did the previous process and when you reboot again happens, did it again and access to your LD. Now install tlp `sudo apt install tlp`. TLP is an improved way of manage your system. Now edit the `/etc/default/tlp` and in the line `RUNTIME_PM_DRIVER_BLACKLIST` add `mei_me`. This should allow you to boot by the force.

# Useful fixes
This repository is where I'll put any fix that is f\*cked up to any problem as f\*cked up as the fix.

It's a personal repository and is not destined to be universal, but feel free to check if you have a problem listed here.

* [Bootloader doesn't launch after Windows update](#bootloader-doesnt-launch-after-windows-update)
* [Civilization VI doesn't work / remove 2KLauncher on Linux](#civilization-vi-doesnt-work--remove-2klauncher-on-linux)
* [WiFi at startup](#wifi-at-startup)


# Bootloader doesn't launch after Windows update

First, make sure you have Secure Boot disabled. Sometimes Windows re-enables it.

If it's still not working, check if your partitions did not change their numbers due to the Windows update *(yes, it happened to me and I still don't know how nor why)*; e.g. boot a live USB and check if your root partition is still nvme0n1p5 by mounting it.

If partitions numbers have been swapped, then you must recreate your boot entries, just as if they disappeared:
1. Boot to a live USB
2. Execute `efibootmgr -v` to get both boot entry's number (`XXXX` in `BootXXXX`) and targeted partition (first number in the long details): normally it should be another partition than what you noted from mounting (e.g. EFI partition, targeted by bootloaders, is "8" instead of "5"). **If partitions are correct, then maybe this is not the root of your problem.**
3. Delete your bootloader's entry:
```sh
# efibootmgr -b XXXX -B
```
4. Recreate it (for HDD/SSD, instead of `nvme0n1` it should be `sdX`). For example, to recreate rEFInd's boot entry:
```sh
# efibootmgr -c -d /dev/nvme0n1 -p <partition number> -L "rEFInd Boot Manager" -l "\EFI\REFIND\REFIND_X64.EFI"
```
5. **Make sure to edit partitions' numbers in your bootloader's config file, in lines like `rw root=/dev/nvme0n1pX ...`).**


# Civilization VI doesn't work / remove 2KLauncher on Linux

Enable Proton, and then set this as starting command in Steam, replacing `/CivilizationVI` with `/CivilizationVI_DX12` if you're using DX12:
```sh
eval $( echo "%command%" | sed "s/2KLauncher\/LauncherPatcher.exe'.*/Base\/Binaries\/Win64Steam\/CivilizationVI'/" )
```


# WiFi at startup

Make sure:
* you have wireless drivers installed
* you have keyrings installed and started at launch

Check in `/etc/xdg/autostart` for some .desktop related to wireless connections that may be useful.

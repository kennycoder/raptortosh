
# "Raptortosh"

This is an EFI with all the necessary kexts for building a 13th gen mini-ITX hackintosh.

It is tailored for the following system build as of March 2024:

 - CPU: Intel® Core™ i7-13700KF (Raptor Lake)
 - Motherboard: ASUS Strix B760-i Gaming Wifi
 - RAM: (Kingston) 64GB DDR5 6000
 - GPU: (Sapphire) Radeon RX 580 8GB
 - NVME: WD_BLACK SN770 1TB

## What works

Everything so far
  
## What doesn't?

Nothing so far

# Contents

## kexts:

 - AirportItlwm.kext
 - AppleALC.kext
 - AppleIGC.kext
 - AppleIntelI210Ethernet.kext
 - BlueToolFixup.kext
 - CPUFriend.kext
 - CPUFriendDataProvider.kext
 - IntelBluetoothFirmware.kext
 - IntelBTPatcher.kext
 - Lilu.kext
 - NVMeFix.kext
 - RadeonSensor.kext
 - RestrictEvents.kext
 - SMCProcessor.kext
 - SMCRadeonGPU.kext
 - SMCSuperIO.kext
 - USBPorts.kext
 - USBToolBox.kext
 - UTBMap.kext
 - VirtualSMC.kext
 - WhateverGreen.kext

## Drivers:

 - OpenCanopy.efi
 - OpenHfsPlus.efi
 - OpenRuntime.efi
 - ResetNvramEntry.efi
 - ToggleSipEntry.efi

## ACPIs:

 - SSDT-AWAC.aml 
 - SSDT-EC-USBX.aml 
 - SSDT-PLUG-ALT.aml

# Necessary setup steps:

- Clone this repo

## Setting the serial information

Get GenSMBIOS [https://github.com/corpnewt/GenSMBIOS], select option 1 to download MacSerial and next option 3 to generate new values. Make sure you chose MacPro 7,1 as the platform

Write down the following generaed values somewhere:

- MLB
- SystemSerialNumber
- SystemUUID
- ROM (read below)

For the ROM I suggest you check your ethernet card's MAC address and use it as your ROM (wihout the colons).

If you MAC address is for example 

> aa:bb:cc:dd:ee:ff

then your rom would be 
> aabbccddeeff

This helps a lot with iServices (nevertheless, your apple ID is the most important part).

## Getting the MacOS recovery files

Get OpenCorePkg [https://github.com/acidanthera/OpenCorePkg/releases]
Go to the folder Utilities/macrecovery and run the following:

- For Ventura (13)

> 	python3 macrecovery.py -b Mac-4B682C642B45593E -m 00000000000000000 download

- For Sonoma (14, latest as of 03/2024) 

> 	python3 macrecovery.py -b Mac-937A206F2EE63C01 -m 00000000000000000 download

## Setting the values in the config.plist

Get OCAuxiliaryTools [https://github.com/ic005k/OCAuxiliaryTools]

1. Open the EFI/OC/config.plists (that you cloned from this repo) with OCAuxiliaryTools, go to "PI" menu, fill in the MLB, SystemSerialNumber, SystemUUID, and ROM (ideally the one created from your network card's MAC address)
2. Save and close

## Creating USB stick

Follow Dortania's guide [https://dortania.github.io/OpenCore-Install-Guide/installer-guide/]

TL;DR:

1. Format the stick correctly, copy the EFI folder from this repo to it
2. Copy the downloaded files from the "Geting the MacOS recovery files" section, the ones that were downloaded with the macrecovery.py command into "com.apple.recovery.boot" folder on the usb stick.

Your USB stick should have the following structure:

> /com.apple.recovery.boot/

> /com.apple.recovery.boot/BaseSystem.chunklist

> /com.apple.recovery.boot/BaseSystem.dmg

> /EFI (folder from this repository)

## Installation

1. Boot from the USB stick, wait a bit until the UI shows up. boot-args in this specific EFI have the -v flag so one can see the output and if something fails along the way.
2. Format your drive (APFS), start the setup. Make sure you are connected to the internet with either enthernet cable or WiFi (yes, it works on this motherboard's Intel card)
3. After the installation is done, install OCAuxiliaryTools on your brand new OS
4. Open OCAuxiliaryTools, mount the EFI partition and open the config.plist, go to NVRAM, remove the '-v' flag from boot-args.
5. (optional yet important) - due to issues with Sonoma, installation is performed with SecureBootMode set to Disabled. Open OCAuxiliaryTools, go to Misc -> Security, change SecureBootMode to Default to avoid issues with iServices.
6. Save, reboot
7. Enjoy your new build (and make sure to save your serials and whatnot)


## Misc

boot-args contain 

> -wegnoigpu

 since this is a **-F** processor (no iGPU). Also, boot-args do **NOT** have 

> agdpmod=pikera

 since I'm on RX 580. If you are on 5000/6000 series, please add it to your boot-args.

  
## Credits

- Obviously, Dortania's guide
   [https://dortania.github.io/getting-started/] 
- r/hackintosh
   [https://www.reddit.com/r/hackintosh/] 
- tonymacx86 [https://www.tonymacx86.com]
- Everyone who wrote the drivers, kexts, acpis, etc -> without you it wouldn't be possible

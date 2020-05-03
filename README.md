# an515-51-hackintosh
Hackintosh for Acer Nitro 5 AN515-51

#### Supports MacOS 10.15.x, 10.14.x

## What's Working...
 - [x] Audio & headphone jack
 - [x] CPU Speedstep (XCPM)
 - [x] iGPU with disabled dGPU
 - [x] Fully Functional QE/CI Enabled Graphics
 - [x] Battery Management
 - [x] Ethernet
 - [x] Internal SD card Reader
 - [x] HDMI + Audio
 - [x] Sleep + Wake
 - [x] Smart Touchpad + Gestures (using I2C)
 - [x] WebCam
 - [x] Usb 3.0 + Type C
 - [x] Sleep From Lid
 - [x] WiFi (2.4 + 5GHz) + BT by using BCM94352z
 - [x] Native hotkey support with Fn keys


## Installation

 ### BIOS Settings
* *Security* → Set supervisor password (to disable secure boot)
* *Security* → Password on boot → **Disable**
* *Boot* → Secure Boot → **Disable**
* *Boot* → Boot Mode → **UEFI**
* *Main* → Touchpad → Set touchpad mode → **Advance**
* *Main* → Lid Open resume → **Enabled**

### Post Installation

- Disable Hibernation : Hibernation (suspend to disk or S4 sleep) is not supported on hackintosh. it could cause problems if you don't disable it.

```sh

$ sudo pmset -a hibernatemode 0

$ sudo rm /var/vm/sleepimage

$ sudo mkdir /var/vm/sleepimage

$ sudo pmset -a standby 0

$ sudo pmset -a autopoweroff 0


```

- If you have Installed MacOS on SSD, Enable TRIM using following command:

```sh

$ sudo trimforce enable

```

### Laptop configuration


- **Audio** : The Sound Card is `Realtek ALC255`, which is driven by `AppleALC` on `layout-id 3`.

- **CPU** : The CPU model is `i5-7300HQ` and XCPM power management is natively supported.
 - - XCPM and HWP are recommended to work together to reach better power management, by injecting `plugin-type=1` with `SSDT-XCPM`, and using `CPUFriend.kext`.

- **Graphics** : The iGPU is `Intel UHD Graphics 620`, and its enabled using `Ig-Platform-id=0x191E0000`.
- - The discrete dGPU is `NVIDIA GeForce MX150`, and it is disabled because macOS doesn't support optimus technology. Plus battery life is much improved.
- - The colour branding or corrupted colour depth is fixed with Intel Skylake spoof and EDID fix.
- - Native brightness hotkey support using `DSDT.aml`.

- **Battery** : Battery Management using `SMCBatteryManager.kext`.

- **Backlight** : Native Brightness control using `SSDT-PNLF.aml`.

- **Ethernet** : Gigabit Ethernet using `RealtekRTL8111.kext`.

- **HDMI** : Intel Framebuffer patches to change the connector-type to match the physical connector, using `WhateverGreen.kext`.
- - **HDMI Audio** : Injecting property `"hda-gfx" = "onboard-1"` on HDAU, IGPU, HDEF objects & also injecting `layout-id 3` on HDAU to match layout-id on HDEF.

- **Touchpad** : Native I2C touchpad support using `VoodooI2C.kext`; Set touchpad Mode to `Advance` from `BIOS` & All gestures will work fine.

- **USB** : Custom `SSDT-UIAC.aml` SSDT for `USBInjectAll.kext` that configures USB ports on XHC such that the port limit patch is not needed, and each UsbConnnector value is correct for each port.
- - You can also find USB port mapping for `SSDT-UIAC` [Here](https://github.com/SiddheshNan/Acer-A515-51G-Hackintosh/blob/master/USB%20port%20mapping%20for%20SSDT-UIAC.md).


- **Wi-Fi** : Stock WiFi Card is `Atheros QCA9377` It is not supported on MacOS.
- - Best Choice will be to replace current Card with `BCM94352Z` which has WiFi+BT, or . You can find it on AliExpress for like $20-30. I Have already changed my current WiFi card with `BCM94352Z (Lenovo's 04x6020)`.
- - This `BCM94352Z` card comes from 2 manufacturer called:
- - - 1. DELL - mainly called `DW1560` - has `A key NGFF notch` - much expensive
- - - 2. Lenovo - mainly called `04x6020` - has `E key NGFF notch` - not so expensive
- - The Dell's `DW1560` is much expensive and is only recommended for laptops with `A key NGFF notch` & also laptops which have whitelisted cards from certain vendors.
- - On the other hand Lenovo's `04x6020` has `E key NGFF` notch and is much cheaper than `DW1560`.
- - **This laptop supports both & you are free to choose between the two.**
- - Keep in mind, this laptop uses **M.2(NGFF)** Socket with **A+E Key**. Half size card won't work.
- - Config.plist is already patched for `BCM94352Z` and `BCM943602BAED` & added kexts for BT as well.
- - **If You Don't Have Compatible WiFi Card Installed then Please visit [without-wifi-patches(BCM94352Z) Branch](https://github.com/SiddheshNan/Acer-A515-51G-Hackintosh/tree/without-wifi-patches(BCM94352Z)) of this Repo.**

- **Bluetooth** : Stock `Atheros QCA9377` BT will work out-of-the box, But you can't turn it off because BT power management is not supported;
- - To Fix this you'll have to get a MacOS compatible WiFi+BT card. The best choice will be `BCM94352Z` which has WiFi+BT.

- **SD Card Reader** : Internal SD Card Support using Modified Sinetek's rtsx Driver.


## Credits

- **Special Thanks** to [Acidanthera](https://github.com/acidanthera) for most of the Kexts.
- **Special Thanks** to [RehabMan](https://github.com/RehabMan).
- Thanks to [Clover Bootloader](https://sourceforge.net/projects/cloverefiboot).
- Thanks to [goodwin](https://github.com/goodwin/) for [ALCPlugfix](https://github.com/goodwin/ALCPlugFix).
- Thanks to [alexandred](https://github.com/alexandred/) for [VoodooI2C](https://github.com/alexandred/VoodooI2C).
- Thanks to [Sinetek](https://github.com/sinetek) for [Sinetek-rtsx](https://github.com/sinetek/Sinetek-rtsx).
- Thanks to [al3xtjames](https://github.com/al3xtjames) for [NoTouchID](https://github.com/al3xtjames/NoTouchID).
- Thanks to [daliansky](https://github.com/daliansky/) for Some Patches which I used here from [XiaoMi-Pro](https://github.com/daliansky/XiaoMi-Pro/).

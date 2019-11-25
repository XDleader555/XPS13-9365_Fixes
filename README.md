# XPS13-9365_Fixes
Collection of fixes to improve user experience.

I am not responsible for any losses or damages caused by the use of these fixes. USE AT YOUR OWN RISK.

## Setup IFR Usage (Hidden bios options)
Setup IFR contains all hidden bios settings. It Requires the use of a modified version of [setup_var](https://github.com/XDleader555/grub_setup_var) to allow the selection of different stores.

Search the IFR file for the setting you want to change and note the VarOffset and VarStore.
Additionally note the availble options for your setting.
```
QuestionId: 0xBE8 equals value 0x0 {12 06 E8 0B 00 00}
One Of: Intel(R) Speed Shift Technology, VarStoreInfo (VarOffset/VarName): 0xB, VarStore: 0x3, QuestionId: 0x20F, Size: 1, Min: 0x0, Max 0x1, Step: 0x0 {05 91 43 01 44 01 0F 02 03 00 0B 00 10 10 00 01 00}
	Default: DefaultId: 0x0, Value (Other) {5B 85 00 00 08}
		Value {5A 82}
			QuestionId: 0x2 equals value in list (0x14, 0x1A, 0x1B) {14 8C 02 00 03 00 14 00 1A 00 1B 00}
				64 Bit Unsigned Int: 0x0 {45 0A 00 00 00 00 00 00 00 00}
				64 Bit Unsigned Int: 0x0 {45 0A 00 00 00 00 00 00 00 00}
				Conditional {50 02}
			End {29 02}
		End {29 02}
	End {29 02}
	One Of Option: Enabled, Value (8 bit): 0x1 {09 07 03 00 00 00 01}
	One Of Option: Disabled, Value (8 bit): 0x0 {09 07 04 00 00 00 00}
End One Of {29 02}
```

In this example, The VarStore is 0x3 and the VarOffset is 0xB. To enable we write a 0x1 to the variable. Likewise, to disable we write 0x0.
At the top of the IFR, you'll find the translations for the VarStores.
```
VarStoreEFI: VarStoreId: 0x3 [B08F97FF-E6E8-4193-A997-5E9E9B0ADB32], Attrubutes: 7, Size: 20B, Name: CpuSetup
```
Here, the VarStore 0x3 corresponds to CpuSetup.

The final command to write 0x1 (Enable) to Store 'CpuSetup' at offset 0xB would be: 
```
setup_var CpuSetup 0xB 0x1
```

## List of common hidden bios options
These options are up to date per BIOS 2.9.0. Refer to your own IFR extract for maximum compatibility.

Enable Speed Shift
```
setup_var CpuSetup 0xB 0x1
```

Disable CFG Lock
```
setup_var CpuSetup 0x3C 0x0
```

Disable HPET
```
setup_var PchSetup 0x18 0x0
```

Disable VT-d
```
setup_var SaSetup 0xE3 0x0
```

## Dell Active Pen Right Click Hover Fix
If the top barrel button on your active pen defaults to hover clicks on reboot, import this registry file to permanently disable it. This prevents accidental right clicks when trying to make selections apps like OneNote.

It is recommended you install the [Dell Active Pen App](https://www.dell.com/support/home/au/en/audhs1/drivers/driversdetails?driverid=56dt7&oscode=wt64a&productcode=dell-active-pen-pn557w) for full configuration.

## Drivers
[Intel Rapid Storage Technology Driver](https://downloadcenter.intel.com/download/29094/Intel-Rapid-Storage-Technology-Intel-RST-User-Interface-and-Driver?product=99745)

Download SetupRST.exe. Replaces the Standard NVM Express Controller Microsoft Driver.
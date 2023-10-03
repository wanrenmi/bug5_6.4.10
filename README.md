# bug5_6.4.10
A kernel bug in au0828_usb_probe in drivers/media/usb/au0828/au0828-core.c

Due to the specific malformed usb descriptor file, the driver vulnerability in au0828 was triggered causing a kernel panic. Under certain judgment conditions, the free operation of the dev value in the function au0828_usb_probe (in drivers/media/usb/au0828/au0828-core.c) may again pass it as a parameter to other functions related to memory operations, resulting in use-after-free vulnerability.
![image](https://github.com/wanrenmi/bug5_6.4.10/assets/42407501/3b6af55f-4152-469e-b7d4-3d87ace4b9a0)
The following is the input usb descriptor file:
![image](https://github.com/wanrenmi/bug5_6.4.10/assets/42407501/495d2c5b-aff6-42a5-9c39-e30bb694f40a)
Among them, the first 18 bytes are the device descriptor, and the latter is the config descriptor. Insert the above file as a real or simulated USB device into a host using the linux kernel (It is currently certain that kernel version <=6.4.10 will be affected by this vulnerability, the latest kernel version has not been tested, but because there is no such part of code update, so there is a high probability that this vulnerability also exists). The result of the vulnerability triggering situation is shown in the figure below:
![image](https://github.com/wanrenmi/bug5_6.4.10/assets/42407501/7c89aa1f-5a27-406b-9ac6-25b8c6fe6b62)
![image](https://github.com/wanrenmi/bug5_6.4.10/assets/42407501/970c62b0-93ec-442a-86bf-f0c25b5d5497)

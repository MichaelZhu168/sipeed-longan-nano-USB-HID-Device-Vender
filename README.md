# sipeed-longan-nano-USB-HID-Device-Vender
board sipeed longan nano or GD32vf103 dev board. chip (risc-v32) GD32vf103CBT6 or GD32vf103VBT6 firmware for a USB HID device (vender type) to send data to USB post (Android or PC)

/*!This project is created by mzhu thru cloning from HID_mouse (project name:usbd_mouse) of GD32vf103 examples.
*/

  The Full Speed (FS) USB module uses a 48MHz clock, which is generated 
from an integrated PLL.

  The GD32 device is enumerated as an USB mice, that uses the native PC Host
HID driver to which the GD32VF103V-EVAL-V1.0  board or (sipeed longan nano)is connected.


  This demo supports remote wakeup (which is the ability of a USB device to 
bring a suspended bus back to the active condition), and the CET key is 
used as the remote wakeup source.

  In order to test USB remote wakeup function, you can do as follows:
    - Manually switch PC to standby mode

    - Wait for PC to fully enter the standby mode

    - Push the CET key

    - If PC is ON, remote wakeup is OK, else failed

I use the NucleiStudio (a customed version of Eclipse), since they already include all the packages like CDT tools for embed programming. I uploaded all the screenshots for all the settings. 
    
1. the Core timer interrupt 500 times /second (setting in systick.c). 
    its handler "eclic_mtip_handler(void)" in gd32vf103_it.c. 
    In this handle, it will send out HID report to the host;
     //--1000 per second may be high for host to consume it
    //--tried. 250 is best result
    //---at 500/second and each report contain 64 bytes,  there are 20% reports will be missed by the host.
    //---at 500/second and each report contain 44 bytes,  there are 18.3% reports will be missed by the host.
    //---at 500/second and each report contain 24 bytes,  there are 12.3% reports will be missed by the host.
    //I used my VC++ program to count it.-----mzhu  3/31/2021
    GD32VF103 chip has only full speed USB (12mbits/s), not a high speed USB(480mbits/s)

2. The USB device VID, PID and HID report descriptor, and the IN Endpoint 1 .bInterval  = 0x01U  is in standard_hid_core.c;

3. the system frequency and PLLs setting are in system_gd32vf103.c. it is 96m Hz now. I tried 108m Hz, but the USB doesn't work. 

4. Created a new HID report descriptor(vendor provided) on standard_hid_core.c. total 24-8bites;

5. On 4/1/2021. Successfuly uploaded to LONGAN NANO borad(which has GD32vf103CBT6 chip) without change any code/compiler setting/debug setting.
	It functions exactly as the GD32vf103 Dev board for the USB purpose. The linker is the same GD32VF103xB.lds for GD32vf103CBT6 chip and GD32vf103VBT6 chip; 

6. 4/15/2021 tried 250/second, it is much better than 500/second regarding the lose of reports caputured by host.

any question? drop me a line. 
Hope you like it.

Note: I can't share the USB HID host VC++ project on the PC side since it is a job related project. but you can find similare ones on internet. like in Microchip or TI libaries.
		

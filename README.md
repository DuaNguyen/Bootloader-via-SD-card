# Bootloader-via-SD-card
GUIDEBOOK : COMPILING PROGRAM WITH BOOTLOADER
1. Step 1: compiling bootloader and loading the program
Bootloader is placed from first program memory address (flash memory). This address is 0x08000000. We have to check linker scripts file of MCU corresponding to platform IO compiler (GCC_ARM). This check ensure that first program memory address is correct.

Linker script file is located at: ..\platformio\packages\framework-mbed\targets\TARGET_STM\TARGET_STM32L4\TARGET_STM32L432xC\device\TOOLCHAIN_GCC_ARM\STM32L432XX.ld 
	
	Information in the file we need to pay attention:
MEMORY
{
  FLASH (rx): ORIGIN = 0x08000000, LENGTH = 256k
  SRAM1 (rwx): ORIGIN = 0x20000188, LENGTH = 64k - 0x188
}
FLASH (rx): ORIGIN = 0x08000000 : this script description first program memory address. Ensuing it’s 0x08000000 before load program into MCU.
2. Step 2: Compiling application program  
In this case bootloader is available in flash memory, we have to shift first address of application program memory in flash. That ensure that bootloader doesn’t overlap application program. Due to size of bootloader is about 83 Kb , first address of application program memory is 0x08015000 ( this address is corresponding to 84kb) .
Thus, we have to edit information in linker script file:  ORIGIN = 0x08015000 before building program. 
3. Step 3: Loading application program into MCU
After we build application program (step 2) we can load the program into MCU (application program will be not overlap bootloader). In addition, we can load application program by bootloader via SD card communication. 
Loading application program by bootloader via SD card communication:
We copy firmware file (file .bin) into SD card. This file was created by building code in Step 2. (file name have to be firmware.bin). next, we insert SD card into socket. After that, pressing reset button on board to update new firmware. We can supervise this processing via serial terminal monitor.
 

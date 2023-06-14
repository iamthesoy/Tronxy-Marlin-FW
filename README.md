## TRONXY have finally unfucked their github and are properly maintaining it. I don't have the time to maintain firmware for a bunch of printers i don't own so i am archiving this repo. Please see TRONXY's github page here: https://github.com/tronxy3d




## Applicable printers
  This firmware is built for the following machines only
  XY Series: XY2-PRO,XY2_PRO_2E,XY2_PRO_3E,XY3-PRO,XY3_PB,XY3-PRO-V2,XY3SE,XY3SE_2E,XY3SE_V2
  X5S Series: X5SA,X5SA_g,X5SA_MINI,X5SA_PRO,X5SA_2E,X5SA_PRO_3E,X5SA400,X5SA400_PRO,X5SA400_2E,X5SA500_2E
  D01 Series: D01,D01_PLUS


## How to compile

  1. Install [vscode](https://code.visualstudio.com/), and install the platformio plugin from vscode extensions.
  2. Download this repository and unzip it, in vscode go to file -> Open Folder, select the firmware folder, and open it.
  3. Check the model of the machine matches one of the supported machines. It should be visible on the printer's info screen.
<img align="center" width=400 src="buildroot/share/pixmaps/tronxy/info.png" />
  <br>
  4. If your model is not supported, add an issue, and the firmware can hopefully be obtained from TRONXY

  5. Open Marlin/TronxyMachine.h, Find #define TRONXY_ PROJ, change the project name to PROJ_ XXX (XXX represents your printer model)
    - Note: The modified project name must be a name defined above the file(TronxyMachine.h). 
   For example this would be your config for a TRONXY XY2_PRO:
<img align="center" width=473 src="buildroot/share/pixmaps/tronxy/modify_model.png" />
<br>
  6. Compile the firmware. The compiled binary is placed in the 'update' folder

  7. Copy the 'update' folder into the root directory of the SD card, insert the card into the printer, restart, and the machine will automatically update the firmware. 

 ## If firmware does not flash: 

By default, the MCU referrenced in this firmware is the STM32F446ZET, but some machines use the clone of this chip (GD32F4).  
You can check the chip type by either checking the system -> info menu on the printer's touchscreen, or look at the motherboard and read the writing on the chip.

   When GD32 series chips are used, you need to modify:
    - In the "platformio.ini" file, find the Section [my_board], where there is a "DMCU_TYPE=0", change it to "DMCU_TYPE=4"
    - In the platformio installation directory, find .platformio\packages\framework-arduinoststm32\cores\arduino\wiring_analog.c, find function: uint32_t         analogRead(uint32_t ulPin),Add two statements under the this 
    
    "value = adc_read_value(p, _internalReadResolution)":value >>= 2;value &= 0x03FF;
    
   <img align="center" width=685 src="buildroot/share/pixmaps/tronxy/gd32_analog.png" />
   <br>
   After this has been changed, recompile and flash. This should fix the issue.
  
  If you encounter problems like " multiple definition of 'EXTI1_IRQHandler' " as shown below, please find the file in the image below
<img align="center" width=685 src="buildroot/share/pixmaps/tronxy/exti_mul_def.png" />
<br>
    open the file and comment out “void EXTI1_IRQHandler(void)”(according to the error message) function, as shown in the image.
<img align="center" width=685 src="buildroot/share/pixmaps/tronxy/comment_exti.png" />
<br>

## FAQ

If you want to switch back to the original Marlin UI, define "#define TRONXY_UI" in "Marlin/TronxyMachine.h" as UI_MARLIN_DEFAULT
  - Note: If you change this, remember what it was before (ideally leave a commented line). Otherwise, if you try to switch back to the stock interface, it     will not work, because different values correspond to different resolution screens.
<img align="center" width=482 src="buildroot/share/pixmaps/tronxy/default_ui.png" />
<br>

 

## Other
This is the latest version of the marlin firmware used by TRONXY. for the latest version of stock marlin, go to [Marlin on github](https://github.com/MarlinFirmware)


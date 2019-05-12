
I started my code from Florian Repository ( https://github.com/flo199213/Hoverboard-Firmware-Hack-Gen2 )

Florian rewrote the firmware for hoverboards composed by 2 boards, starting from Niklas repository ( https://github.com/NiklasFauth/hoverboard-firmware-hack/ )

Niklas rewrote the firmware for hoverboards composed by a big board and sensor boards.

First thing that I've done was to add PWM input to control the robot with a RC receiver, in order to let me do all the functional checks, before to implement automatic navigation algorithm.
I used turnigy rc receiver directly connected to the board, on the unused gpio.

Second thing that i've done was to add PWM output in order to control a ESC connected to a motor. The motor is connected to a blade. 

Every time the robot moves, the blade starts to rotate at constant speed.

Third thing that I. ve done was to make the mechanical project (you find it in the folder LawnMowerMEchanicalProject).
These are some images (the base is aluminium and the cover in polycarbonate):
![otter](https://github.com/gaucho1978/LAWNMOWER-ROBOT-from-Hoverboard-/blob/master/LawnmowerMechanicalProject/3D%20PROJECT%20-%20INVENTOR%202014/pictures/bottom1.png)
![otter](https://github.com/gaucho1978/LAWNMOWER-ROBOT-from-Hoverboard-/blob/master/LawnmowerMechanicalProject/3D%20PROJECT%20-%20INVENTOR%202014/pictures/side.png)
![otter](https://github.com/gaucho1978/LAWNMOWER-ROBOT-from-Hoverboard-/blob/master/LawnmowerMechanicalProject/3D%20PROJECT%20-%20INVENTOR%202014/pictures/top.png)
![otter](https://github.com/gaucho1978/LAWNMOWER-ROBOT-from-Hoverboard-/blob/master/LawnmowerMechanicalProject/3D%20PROJECT%20-%20INVENTOR%202014/pictures/front.png)


------------------------------
#### MECHANICAL CONSIDERATIONS
- You will find files for manufacturing alluminium plate and polycarbonate cover in the folder:
   LawnMowerMEchanicalProject/FILES FOR MANUFACTURING\PDF FORMAT
- You will fing files for manufacturing plastic supports with 3d printer in the folder:
   LawnMowerMEchanicalProject/FILES FOR MANUFACTURING\3DPrinted supports
- The blade support shall be 3d printed, then 4 cutter blades are screwed on the 4 edges of the plastic support. You could also purchase a blade for lawnmower online.
- The shaft to fix the blade is obtained modifing a screw.
- The shaft is maintained in its vertical position through bearings. 
- The bearing is mainteined in its position through a 3d printed support interfacing bearing and the alluminium plate.
- I used autolock bolts to fix the blade on the shaft.


------------------------------
#### BILL OF MATERIAL - TOTAL COST 120 EURO
50 EURO - USED HOVERBOARD - https://www.mediaworld.it/product/p-742077/i-bike-kidplay-streetboard-one-hoverboard-6-5?ds_rl=1250284&ds_rl=1250284&gclid=CjwKCAjwiN_mBRBBEiwA9N-e_qMfsYSRb5w4XxsWQmA6IEDfbXj_eAiy6rWeXzdMz_envhganzewVxoCpBMQAvD_BwE&gclsrc=aw.ds

6 EURO - STLINK V2 PROGRAMMER (TO FLASH HOVERBOARD FIRMWARE) - https://www.ebay.it/itm/352654456738?ViewItem=&item=352654456738

5 EURO - FIVE PROXIMITY SENSORS HC-SR04 - https://www.ebay.it/itm/312495306039

2 EURO - Arduino nano(clone): https://www.ebay.it/itm/322913230315

1 EURO - DCDC(FOR ARDUINO AND/OR BLADE MOTOR): https://www.ebay.it/itm/122201239217

10 EURO - MOTOR FOR BLADE (I REUSED MOTOR+ESC FROM A DJI S1000 MULTICOPTER, BUT YOU CAN BUY USED MOTOR FROM ELECTRIC WINDOWS OF A CAR AND ADAPT IT)

10 EURO - TWO 3D PRINTED SUPPORTS FOR BEARING, ONE 3D PRINTED SUPPORT FOR BLADE, AND ONE 3D PRINTED SUPPORT FOR BOLT CAP TO BE MOUNTED CLOSE TO THE BLADE  (I PRINTED THEM FOR FREE WITH MY PRINTER)

1 EURO - FOUR BLADES FOR CUTTER (I FOUND THEM IN A LOCAL HARDWARE STORE (OBI))

10 EURO - POLYCARBONATE OR PLEXIGLASS COVER (I FOUND PIECE OF POLYCARBONATE FOR FREE )

10 EURO - ALUMINIUM OR IRON BASE (I FOUND ALUMINIUM PLATE FOR FREE)

5 EURO - SCREWS AND BOLTS

5 EURO - TWO ROTATING WHEELS 66mm HEIGHT (I FOUND THEM IN A LOCAL CHINESE SHOP)

2 EURO - PANIC BUTTON - https://www.ebay.it/itm/Rosso-fungo-Cap-1-NO-1-NC-ferma-emergenza-Pulsante-interruttore-DPST-660V-10A/123270871363?ssPageName=STRK%3AMEBIDX%3AIT&_trksid=p2057872.m2749.l2649

2 EURO - TWO BEARINGS (I REUSED BEARING FROM A INLINE SKATE WHEEL)


------------------------------
#### HOVERBOARD HARDWARE
The hoverboard with 2 boards uses processor GD32F130C8 (instead of STM32F103 used on Niklas hoverboard) 
![otter](https://github.com/gaucho1978/LAWNMOWER-ROBOT-from-Hoverboard-/blob/master/HoverboardPCBFirmware/images/Hardware_Overview_small.png )

The hoverboard hardware has two main boards, which are different equipped. They are connected via USART. Additionally there are some LED PCB connected at X1 and X2 which signalize the battery state and the error state. There is an programming connector for ST-Link/V2 and they break out GND, USART/I2C, 5V on a second pinhead.

The reverse-engineered schematics of the mainboards can be found in the folder HoverboardSchematics

In order to use  GPIO represented as "not used" on the hoverboard schematic, you need to solder some zero ohm resistors (or jumpers) (see schematic).

I soldered the resistive divider(see lawnmower schematic) directly on the pcb, on the free pads related to  missing components. this way the connection to the rc receiver becomes simple as few wires. 

I REUSED THE HOVERBOARD HARDWARE AS IT IS. I DIDN'T MODIFIED THE FRAME, I JUST ROTATED IT 180 DEGREES.


------------------------------
#### HOVERBOARD FIRMWARE
the following image shows how the 3 phases changes during rotation. 

Note: A complete rotation of the phases is not a complete rotation of the wheel since there are many inductors inside the motor. 
![otter](https://github.com/gaucho1978/LAWNMOWER-ROBOT-from-Hoverboard-/blob/master/HoverboardPCBFirmware/images/Raumzeigerdiagramm.png )

THE FILE "DEFINES" AND "CONFIG" CONTAINS MAIN PARAMETERS 
THE FILE "IT" CONTAINS TIMING FUNCTIONS AND INTERRUPTS (PERIODIC CHECKS,  SERIAL LINE INTERRUPTS, SYSTEM TICKS)
THE FILE "BDLC" CONTROLS THE HOVERBOARD MOTORS
THE FILE "COMMSSTEERINGPWM" RECEIVES THE RC PWM SIGNALS TO MOVE THE HOVERBOARD IN SPEED AND DIRECTION.
THE FILE "COMMSMASTERSLAVE" SENDS AND RECEIVE DATA BETWEEN MASTER AND SLAVE BOARD
THE FILE "LED" CONTROLS THE GPIO OUTPUT GENERALLY CONNETED TO LEDS TO SHOW BATTERY STATUS ETC,ETC.
THE FILE "COMMSACTUATOR"GENERATES THE PWM USED TO CONTROL THE MOTOR CONNECTED TO THE BLADE.
THE FILE "MAIN" IS THE MAIN LOOP OF THE HOVERBOARD FIRMWARE THAT CALLS ALL THE ROUTINES IN SEQUENCE.



------------------------------
#### FLASHING
The firmware is built with Keil (free up to 32KByte). To build the firmware, open the Keil project file which is includes in repository. On the board, close to ARM processor, there is a debugging header with GND, 3V3, SWDIO and SWCLK. Connect GND, SWDIO and SWCLK to your SWD programmer, like the ST-Link V2.

- If you never flashed your mainboard before, the controller is locked. To unlock the flash, use STM32 ST-LINK Utility or openOCD.
- To flash the processor, use the STM32 ST-LINK Utility as well, ST-Flash utility or Keil by itself.
- Hold the powerbutton while flashing the firmware, as the controller releases the power latch and switches itself off during flashing

NOTE: I USE TO UNPLUG THE HOVERBOARD BATTERY AND TO PLUG THE STLINK PROGRAMMER (INCLUDED 3,3VOLT PIN), THEN TO FLASH THE FW USING KEIL. THIS WAY YOU DON'T NEED TO LEAVE PRESSED THE POWER BUTTON.

NOTE2: ONE BOARD IS MASTER THE OTHER IS SLAVE. BEFORE TO FLASH THE FW, SET THE FIRMWARE AS MASTER OR SLAVE IN THE CONFIG.H AND RECOMPILE THE FIRMWARE.

------------------------------
#### COMMUNITY
there is a Telegram group as a free discussion platform about Hoverboards MODS. You can find it here: https://t.me/joinchat/BHWO_RKu2LT5ZxEkvUB8uw


------------------------------
#### NEXT THINGS THAT I WILL DO: 
- add sensors and interlocks to algorithm,
- create a docking station for automatic recharge, 
- realize the mechanical structure, 
- implement safety for automatic stopping blade 
- implement automatic navigation algorithm.


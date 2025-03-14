# What is KiloCam? 
I developed KiloCam while I was a PhD student at the University of Hawaii. **In the Hawaiian language to "kilo" means to observe or monitor an ecosystem deeply, usually over long time periods and in the spirit of stewardship.** KiloCam is intended as a tool to help more people steward the environment around them. 

Practically speaking, this repository provides code and assembly instructions for KiloCam as a solder-free alternative to my other camera (CoralCam, https://www.sciencedirect.com/science/article/pii/S2468067219300537). KiloCam is low-power, camera designed to fit into a standard GoPro Hero4 underwater housing, and runs reliably off of any battery from 3.7 - 8 V. To date, KiloCam has been used to monitor coral reefs, agriculture, streams, plant phenology, tidal flats, marine predators, bird nests and more, all by users of many ages. 

# NOTE: Coral-Spawning Users
There has been unprecedented demand for a low-power and relatively low-cost camera for monitoring nocturnal coral spawning. Working with multiple groups we've created a KiloCam-based system to accomplish this. If you are looking for instructions on how to program and use your coral-spawning camera, please scroll to the bottom of this page just before the revision history. 

## Building Your KiloCam
For a video build guide of how to assemble and code your KiloCam, check out the video linked below. Be sure to check the video description for additional notes and always use the most up-to-date code files available in this repo. **As this video is a couple years old, I recommend reading the guide below, and using the video as a visual reference only.**

Build video link: https://www.youtube.com/watch?v=vf54ca9IuP4&t=11s

### What you will need: 
- A KiloCam PCB (https://www.ecologisconsulting.com)
- An ESP32-Cam PCB
	- Note: Two varieties exist. Know if the one you buy comes with a separate programming board, or if you need to buy a USB-to-serial FTDI programmer. ESP32-Cam units that come with an attached programming board cannot be programmed with the FTDI programmer you'll use to program the KiloCam PCB.
- A USB-to-serial FTDI programmer and five female-female jumper cables
- A CR1220 coin cell battery
- A battery (3.7 V - 8 V) and a matching connector. If using a GoPro underwater housing, these work well: https://www.ebay.com/itm/196498098699?itmmeta=01J5DS43H8NEFS5NGT9644TM7Z&hash=item2dc0330a0b:g:r~gAAOSwcuVfl8Rf
- A microSD card formatted to FAT32
	- Note: Because of file system limitations of the ESP32-Cam, cards larger than 4GB will only save up to 4 GB of information. Each photo is about 100 kb, so that is still **a lot** of photos.
- A small flathead screwdriver
- A laptop to program with

### Preparing your lap top: 
1. Install the Arduino IDE (https://www.arduino.cc/en/software)
2. In the Arduino IDE, install these libraries: 
	- From the IDE Library Manager: Time
 	- From this repository: RTClib-Master (this is an older version of RTClib, the newer (https://github.com/adafruit/RTClib) version works too, but is missing some examples)
	- From GitHub: https://github.com/JChristensen/DS3232RTC
	- From GitHub: https://github.com/rocketscream/Low-Power
3. Add board definitions for the ESP32-Cam to your Arduino IDE by following this short guide: https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/
	- **IMPORTANT NOTE:** Certain versions of the ESP32 board definitions (arduino-esp32) are known to cause issues with SD card mounting and camera performance on the ESP32-Cam. When installing the "esp32" board definitions in the Arduino Board manager please only use version 2.0.4 from the drop down menu.
4. Ensure your laptop recognizes your FTDI programmer
	- **With the FTDI unplugged** check under Tools > Port in the Arduino IDE. 
	- **Plug in the FTDI** and check again under Tools > Port again. You should see a new serial port as available. If you do not, follow this troubleshooting guide: https://learn.sparkfun.com/tutorials/how-to-install-ftdi-drivers

### Programming the ESP32-Cam:
As of March 2024, with KiloCam v3 boards (blue in color) require that the ESP32-Cam is programmed only once. All user settings are adjusted simply by programming the KiloCam board, which communicates these settings to the ESP32-Cam. KiloCam v2 PCBs (green in color) require both boards are programmed as they lack this communication (see revision history below). 

1. With the FTDI programmer unplugged, connect to your ESP32-Cam
	- Ensure the jumper on your FTDI programmer is set to 3.3 V
	- Connect female-female jumper wires to the VCC, GND, RX, and TX pins on the FTDI programmer. 
	- Connect the other end of these cables to the ESP32-Cam header pins. 
		- FTDI GND to ESP32-Cam GND
		- FTDI VCC to ESP32-Cam 3.3V
		- FTDI RX to ESP32-Cam TX
		- FTDI TX to ESP32-Cam RX
	- Then, connect a single jumper cable to both IO0 and the GND pin next to it on the ESP32-Cam. This will put the ESP32-Cam into programming mode. 
2. Open the Arduino IDE and the code for your ESP32-Cam. As of March 2024 the most recent code file is "KiloCam_V3_LT_ESP32Code_03042024.ino". 
3. Select Tools > Board > esp32 > AI THINKER ESP32-CAM
4. Connect the FTDI programmer to your laptop, and select it under Tools > Port
5. Press the reset button on your ESP32-Cam to ensure it is in programming mode
6. Press the upload button in the Arduino IDE
7. The sketch should begin uploading within a minute or two. When the Arduino IDE says "Upload Complete" disconnect the ESP32-Cam from all jumper cables and remove these cables from the FTDI programmer. 
8. Insert a newly-formatted microSD card into the ESP32-Cam and ensure the camera cable is connected properly (they are often shipped separately). Your ESP32-Cam is now ready to use. 

### Programming the KiloCam real time clock:
Accurately programming the real time clock on the KiloCam PCB is critical for reliable operation. You should only have to do this process once, but you can easily repeat the steps below any time you notice inaccurate timing or want to change what timezone your KiloCam thinks it is. 
1. Insert a CR1220 battery into the coin cell holder on the KiloCam PCB. The flat side of your battery stamped with "CR1220" should be visible when inserted. Be careful not to damage the plastic retaining clips on the holder. 
2. In the Arduino IDE, open the sketch Examples > RTClib > Settime
3. Select Tools > Board > Arduino AVR > Arduino Pro or Pro Mini
	- In addition, select "ATMEGA328, 3.3V, 8MHz". This is the voltage and clock speed of the KiloCam board. 
4. Identify the holes on the KiloCam PCB that are labeled "FTDI". Each one of these holes matches the pins on your FTDI programmer, but the programmer must be inserted in the correct orientation. **Take your time to ensure you understand how these align**. 
5. Insert the pins of the FTDI programmer into the KiloCam PCB programming holes, and hold the boards at a slight angle to one another to keep these pins under tension and with good contact. 
6. While holding the programmer under tension, press the Upload button in the Arduino IDE. You should see lights on your FTDI programmer flash rapidly when the upload is in progress. 
7. When the Arduino IDE reports "Upload Complete" press the Serial Monitor button in the upper right corner. Set the baud rate to 57600, and then press the reset button on the KiloCam board. You should see the board communicate the current date and time via the serial monitor, matching that of the programming computer. 
8. Congratulations, your real time clock is now programmed. As long as the onboard CR1220 remains in place and has charge, KiloCam will not forget the date or time, even between deployments when disconnected from power. 

### Programming KiloCam: 
The steps below are likely to be repeated for any given deployment. In the KiloCam code you can adjust:
- How many photos are captured in each capture cycle (longer bursts = less battery life)
- When KiloCam wakes for its first capture
- How long KiloCam waits between capture cycles
- What hours of the day KiloCam collects imagery (dawn and dusk values)
	- Note: Advanced users can also set a separate capture interval for night operations, between dusk and dawn. See comments in code. 

1. Open the program file for KiloCam operations. As of March 2024 this is "KiloCam_V3_LT_03042024.ino"
2. Navigate to about Line 31, where you'll see a section commented as user settings. 
3. Input your user settings for how you'd like KiloCam to operate. **Do not alter any code below this section unless you are very confident in doing so - it may change KiloCam's behavior.**
4. Select Tools > Board > Arduino AVR > Arduino Pro or Pro Mini
	- In addition, select "ATMEGA328, 3.3V, 8MHz". This is the voltage and clock speed of the KiloCam board. 
5. Insert the pins of the FTDI programmer into the KiloCam PCB programming holes, and hold the boards at a slight angle to one another to keep these pins under tension and with good contact. 
6. While holding the programmer under tension, press the Upload button in the Arduino IDE. You should see lights on your FTDI programmer flash rapidly when the upload is in progress. The Arduino IDE will report "Upload Complete" when the KiloCam board is programmed successfully. 
7. Congratulations, your KiloCam is now ready to assemble and use. 

### Assemble KiloCam: 
1. Add a connector for your chosen power source to the KiloCam PCB using the screw terminals on the KiloCam PCB. Loosen these using a small flathead screwdriver, insert the cables, then tighten each screw down. **Ensure your cables match the polarity listed on the underside of the KiloCam PCB**.
	- After tightening, give these cables a light tug to ensure they are properly secured. 
2. Stack the ESP32-Cam on top of the KiloCam board. The outer profiles of these boards should align perfectly. 
3. If you haven't already, insert a microSD card formatted as FAT into the ESP32-Cam, and carefully insert the provided camera into the clamshell connector on this board. 
4. Attach your power source (3.7 - 8 V) to the power connector on the KiloCam board. KiloCam will do a few things: 
	- First, KiloCam will immediately run a demo cycle of your code. You'll see two flashes of the KiloCam LED to indicate the start of this cycle, and two flashes of this LED to indicate the end of the cycle. A typical cycle with an empty microSD card takes just a few seconds. If this takes longer than 10 seconds, something is wrong, most likely with the ESP32-Cam microSD card or camera connector. 
	- Second, after performing its demo cycle, KiloCam will enter a low-power sleep until the initial wakeup you programmed. If you programmed an initial wakeup for 8AM, and you power up your KiloCam at 10AM, KiloCam will wait until 8AM the next day to begin operations. 
		- **After KiloCam has blinked twice to indicate the demo cycle is done, and you see all LEDs are powered off, this is a great time to gently remove the microSD card and ensure your images were captured successfully.** On KiloCam v3 boards running the most recent code, each image will have a filename like "IMG_20240303112829_L0_T17.jpg" where the first string is a date and time, the number after L indicates relative light levels from the onboard light sensor, and the number after T is the temperature in celsius as an integer. 
			- Note: Only the most recent v3 boards have light and temperature sensors. These boards are marked with "LT" in the corner. Don't worry, if your board is an older v3 model it will still work just fine and will populate L and T with zeros. 

5. That is it, your KiloCam is ready to use! I reccomend placing it into a GoPro Hero4 underwater housing or similar for safe, reliable deployments in virtually any environment. **A few fun tips:**
	- There is actually room in these underwater housings for two 500 mAh 3.7 V lipo batteries. Stack them in there and use a parallel battery connector to double your deployment time. 
	- Doing extended deployments underwater? Add a tempered glass screen protector to the housing front window to prevent damage from marine biofouling organisms. 
	- Have extra room in your housing? A piece of foam, or even foam earplugs, works great to prevent wobble. Just make sure nothing presses on the CR1220 battery or KiloCam can lose its timekeeping. 
	- **Want a wider field of view?** Version of the OV2640 camera used on the ESP32-Cam are widely available that include 120-degree and 160-degree lenses. These will dramatically improve picture quality over the stock lens. 
	- **Are you a really advanced user?** The ESP32-Cam has all the components necessary for wireless image transmission, and the KiloCam board has breakouts for two digital pins. Use these to add a motion sensor or control an external light source or motor. For high-current applications, use these breakouts to control an external MOSFET switch. 


 ## Building Your KiloCam Spawning-Camera
Follow the written instructions below, or follow this video: https://youtu.be/gGp1QWvjenc
 
When you recieved your spawning camera it should include the following: 
- A KiloCam PCB and ESP32-Cam PCB. Note: The ESP32-Cam PCB is already programmed and you will not need to follow the steps above in this readme unless you want to reprogram it. The KiloCam PCB IS NOT pre-programmed unless otherwise arranged.
- A BlueRobotics 11.8" underwater housing complete with an acrylic front endcap (with camera installed), a 3D-printed chassis, and a rear endcap with a Lumen underwater LED installed.
- Accompanying 3D-printed components such as a bracket for the Lumen LED as well as a pre-installed glass protector over the front acrylic window. Install your Lumen bracket using the included M3 stainless screws if you want it attached to the housing and do this before proceeding. If mounting the Lumen on your underwater housing, put a thin ring of electrical tape around your acrylic window edges to prevent light scattering in your photos. 

You will need to source the following: 
- A Zeee 7.2V 5000mAh NiMH Battery with Tamiya Plug (6-Cell). Insert this battery into the rear of your 3D-printed chassis, with the plug facing out. No specific orientation. Insert the battery fully into its chassis. 
- A microSD card
- A CR1220 coin cell battery (follow instructions above on this page to program your KiloCam real-time clock. You only need to do this once. 

A few notes: 
- Read all instructions below before assembling your camera. DO NOT disassemble your spawning camera before reading all steps below. 

To assemble your spawning camera: 
- Remove the 3D-printed chassis and acrylic endcap by first removing the white locking cord that hold this endcap in place, then gently pulling the endcap and chassis apart from the underwater housing tube. Use BlueRobotics-approved Molykote lubricant (included with your camera) on the provided o-rings and install two o-rings on this endcap.
- Remove the white locking cord that holds the read endcap in place (the endcap with the attached LED). Gently remove this endcap, add two lubricated o-rings, and reinstall the endcap and locking cord. In general use you should not remove this endcap except to service o-rings. 
- Fully charge your NiMH battery with a NiMH-compatible charger. These reccomended batteries are 6-cell batteries (6S) and should charge to 8.4V when completely full. They are falsely advertised as 5000 mAH and are closer to 3000 mAH based on my testing. 
- Follow all instructions above to program your KiloCam real-time clock and KiloCam PCB, but using the files included here in the "Code Files for Spawning Cameras.zip" file. Note that I have also included most required libraries in this folder and these can be installed via the Arduino IDE Sketch -> Include Library -> Add .zip Library menu option.
	- Note: The code for spawning cameras is slightly different and includes both a RunCamera and a RunCamera_LED function. By default, this code does not capture any photos during daytime and only runs the RunCamera_LED function between the dusk and dawn hours set by the user. To capture non-LED images during the day simply uncomment this function near Line 126, but note that doing so will greatly reduce battery life. The default code is programmed to capture 4-image bursts every five minutes from the hours of 8PM to 2AM.  
- After programming your KiloCam PCB, insert the microSD card into your ESP32-Cam PCB and attach this to your KiloCam PCB. Arrange the power wires and signal wire on your KiloCam PCB so they route towards the battery compartment of the 3D-printed chassis. Your spawning camera likely came with the KiloCam and ESP32-Cam boards pre-installed, note how they are wired and that the assembled KiloCam is inserted and removed from the 3D-printed chassis at an angle. Always remove the camera ribbon wire before removing the KiloCam from the chassis - this is done via the small black clamshell attachment point on the ESP32-Cam board. 
- Note the wiring harness that extends from the rear LED endcap through the underwater housing and out the front of the housing. From this wiring harness attach the female LED signal wire (a single "jumper" style wire) to the similar LED signal wire that extends from the KiloCam board. 
- Next, attach the small spade-style connector from the housing wiring harness to the KiloCam board.
- Last, attach the large Tamiya-style connector between the NiMH battery and the housing wiring harness. The spawning camera will immediately run a demo cycle by capturing four LED-illuminated images if programmed with the default Arduino sketch.
- Gently arrange the camera wires so the Tamiya connectors sit in the battery chassis slot, then gently insert the assembled and powered chassis into the underwater housing. Rotate the chassis left and right small amounts to encourage this process until it is firmly seated into the housing.
- Insert the white locking cord at the front end of the acrylic housing to completely seal the underwater housing. The housing IS NOT water tight without the use of all o-rings and the locking cords. Inspect that all o-rings have a good seal. Your spawning camera is now ready to use. 
- Suggestion 1: Wrap the outside of your housing in electrical tape to limit damage from biofouling over long-term deployments.
- Suggestion 2: The spawning camera uses a 45 mm tempered glass smartwatch screen protector to protect its front acrylic window. If this becomes scratched, replacements are easily found on Amazon.
- Suggestion 3: DO NOT use any lubricants other than the included Molykote provided by BlueRobotics. Other lubricants can degrade the acrylic housing, leading to failure, or cause o-rings to swell to the point where you can no longer open your housing. 

## Revision History
- ### July 2024: Code files and assembly instructions provided for spawning-camera variants
	- To support the growing use of KiloCam for work related to coral spawning, I have included written instructions above as well as spawning-specific code files. 

- ### March 2024: Major Code Update for V3 Boards 
	- I have continued to revise and improve the code for KiloCam, as well as the boards themselves. The newest code was released today (March 3, 2024) and represents a considerable improvement. Users can now just program the ESP32-Cam once with the supplied code file "KiloCam_V3_LT_ESP32Code_03042024.ino", then program the time on their KiloCam board, and then upload the file "KiloCam_V3_LT_03042024.ino" to their KiloCam board. All user settings, like the timing and number of photos to capture, are now adjusted towards the top of the KiloCam file and are communicated to the ESP32-Cam by the KiloCam board. Upon wake, KiloCam tells the ESP32-Cam how many photos to take, and for each photo the ESP32-Cam requests a new data packet, including timestamp, from the KiloCam board. These changes: 
		- Make the code easier to use: Upload the ESP32-Cam code to that board just once, then make all other changes (including number of photos per burst) in the main KiloCam code file. 
		- Allow for each image to have its own unique timestamp. This is important when the increment between photos within a single burst increases over time as the number of images on the SD card reaches into the thousands. 
		- If controlling an external LED with KiloCam, like the BlueRobotics Lumen, you can easily modify this code to turn the LED on only when a photo is being captured, not when it is being saved. Contact me if you want example code - this is a common operation with KiloCams used to monitor coral spawning. 
		- Future KiloCam v3 PCBs (the "LT" version) will come standard with integrated light (L) and temperature sensors (T). This code automatically collects average values from these sensors and adds them to the filename for each photo alongside the timestamp - new values are collected for EACH photo. The code will work with V3 boards that do not have these sensors. On future boards a file that reads "IMG_20240303112829_L297_T17.jpg" will represent a photo collected at a light level of 297 (0 is a dark room, 60 is a dimly lit room), and a temperature of 17 degrees celsius. These data are intended to be supplementary, but could be used to guide intelligent sampling strategies by advanced users. 

- ### January 2022: KiloCam V3 Boards Available: 
	- Starting in January 2022 KiloCam V3 boards became available. These boards use serial communications between the ESP32-CAM and the KiloCam PCB to 1) allow automatic timestamping of images with the time and date of the KiloCam RTC, and 2) improve overall power efficiency by letting the ESP32-CAM tell KiloCam when an image has been saved. 
	- KiloCam V3 boards can be used with any ESP32-CAM as long as it is running the appropriate code file. The appropriate code files are: 
	- For KiloCam V3 use "KiloCam_V3_KiloCamCode_DATE": To be installed onto the KiloCam PCB using an FTDI programmer set to 3.3V and the Arduino IDE set for an "Arduino Pro Mini 3.3V 8MHz". 
	- For ESP32-CAM use "KiloCam_V3_ESP32Code_DATE"
- ### Legacy KiloCam V1 and V2 Notes: 
	- Ensure you are using the correct code files for your boards as these differ significantly from KiloCam V3 code files. The appropriate code files are: 
	- For KiloCam V1 and V2 PCBs use "KiloCam_V2_KiloCamCode_DATE": To be installed onto the KiloCam PCB using an FTDI programmer set to 3.3V and the Arduino IDE set for an "Arduino Pro Mini 3.3V 8MHz". 
	- For ESP32-CAM boards used with KiloCam V1 and V2 use "KiloCam_V2_ESP32Code_DATE"

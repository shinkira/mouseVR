# Harvey Lab Mouse VR
Developed by members of the Harvey Lab (http://harveylab.hms.harvard.edu/) and the HMS Research Instrumentation Core (http://instrumentation.hms.harvard.edu/). Community contributions to any aspects of the designs or documentation very welcome, please submit a pull request with a clear description of the changes/additions and the rationale behind them.


## Table of Contents

1. [Overview](#overview)
2. [Main Components](#main-components)
3. [Getting started](#getting-started)
4. [Build Guide](#build-guide)
5. [Screen Assembly](#screen-assembly)
6. [Ball Cup Assembly](#ball-cup-assembly)
7. [Ball Sensor Assembly](#ball-sensor-assembly)
    - [PMW sensors](#pmw-sensors)
    - [ADNS sensors](#adns-sensors)
8. [Ball Sensor Software](#ball-sensor-software)
    - [PMW sensors](#pmw-sensors-1)
    - [ADNS sensors](#adns-sensors-1)
9. [Reward system](#reward-system)
10. [Behavior PCB Overview](#behavior-pcb-overview)
11. [Software](#software)
12. [Headfixation](#headfixation)
13. [FAQs](#faqs)
14. [Appendix](#appendix)


## Overview

### Main components
Harvey Lab miniaturized mouse VR rig for virtual navigation and decision-making tasks

The VR setup is comprised of several independent assemblies:

1. **The screen assembly:** a laser projector projects onto a parabolic screen surrounding the mouse. This is the basis for the visual virtual reality.

2. **Ball cup assembly:** an air-supported 8" styrofoam ball that the mouse can run on, with associated ball cup, sensors, and electronics

3. **Reward delivery system and lick sensor:** lick spout, liquid reward reservoir, solenoid, and associated electronics

4. **Enclosure:** A box surrounding the behavioral setup.

Each of these components is independent of the others: i.e. just the screen could be used in combination with a different treadmill and reward delivery system. The electronics for the ball sensors, reward delivery, and lick detection are all mounted on the same PCB. If only one or two of these functions are needed, you do not need to populate the entire PCB.

The screen assembly is designed to be small enough to be mounted within a standard 19" server rack, which could easily fit 3 rigs stacked vertically (or two + monitor and keyboard station). 

Screen and ball cup assemblies:

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/VROverview.png)


### Getting started

All the parts for the assemblies are listed in this google sheet: https://docs.google.com/spreadsheets/d/1hkXaeRBd7GJPuhIFjeKkEIziQik9mlkZpzjhxGrkA7A/edit?usp=sharing 

The build guide (below) contains detailed schematics of the various assemblies, **as well as additional notes about part requirements and alternative parts**. 

### Getting help

Open an issue. We will do our best to support hardware-related issues and correct errors or update documentation where things are unclear. Issues related to the design and execution of virtual environments (for example running ViRMEn code) is currently beyond the scope of this repository. 

### Contributing

We welcome contributions including, but not limited to, hardware improvements, example code for hardware interfacing, and refinements to our documentation. Please submit your contributions through a pull request.

# Build Guide

## Projector


Update 2021-02-18: 

This back projection screen assembly was originally designed for use with the Celluon Picobit laser projector (released in 2016). This projector (along with our recommended alternative laser projectors) has been discontinued and Celluon no longer produces laser projectors. The advantage of laser projectors is that they can project onto a curved surfaces with the image in focus across the entire surface. We can no longer recommend these however due to their inavailability.

We now recommend using the Texas instruments LightCrafter DLP 3010 EVM:  https://www.ti.com/tool/DLPDLCR3010EVM-G2. The DLP 2010 may also work. The back-projection enclosure has now been adapted for the DLP 3010, with a 3D-printed part holding the projector at a downward angle above the top acrylic piece to account for the upward projection angle. See the acrylic design files suffixed with "DLP_mount" and the "DLP_mount_quarter20" Inventor and STL files for the 3D-printed part, which can be mounted on the top acrylic piece with two 1/4-20 screws.

**Note 1**: The side acrylic pieces in vrRig_laserCutter_allParts_DLP_mount are also modified - they are 2 inches shorter than in the original design, and the slots for the mirrors are moved (only in the y direction). 

**Note 2**: You may have to widen the slot in the 3D-printed part by a fraction of a millimeter to account for projector manufacturing errors. The total width should be 0.4 mm wider than the width of the projector base. The design width of the base is 135.0 mm; you can measure yours with a caliper. If it fits correctly, the projector should slide in under the lips such that it is flat against the base and flush with the back of the mount.

The LightCrafters will be supported for longer than typical consumer projectors, have greater programmability, and have some some nice user features such as the ability to easily display calibration images and independently shut off Red green and blue LEDs in the software, which could be great for imaging applications - getting rid of that hard-to-block red light without a filter. Some groups have also added LEDs of desired wavelength - for example UV (this was using DLP3010). They also have fairly low input lag: from coarse tests it appears to be comparable to a standard dell desktop monitor (5-15 ms), although if this is important for you please conduct your own tests. 

The differences in focus across the screen are fairly subtle even to the human eye, so we do not expect this to impact visually-guided navigation or studies of mouse visual responses. Please see appendix for pictures of a modified screen assembly used with a DLP3010 (Oleksandr Radomskyi, Santiagio Rompani Group, EMBL, Rome). 

Thanks to Oleksandr Radomskyi and Santiagio Rompani for sharing information about using Lightcrafters with these designs. 


OLD INFO - see update above:

The projector should be a laser projector so that the image is in focus across the entire curvature of the screen. In terms of laser projectors compatible with this design, we like the Celluon PicoPro, Celluon PicoBit, or the LaserBeamPro (in that order of preference). The LaserBeamPro can exhibit burn-in.
Avoid the Sony portable laser projector due to lag. The projector throw ratio matters: get one with 1.4:1 (pretty standard for
the small laser projectors). Projectors with greater divergence will work as well. Get a projector that projects straight out i.e.
the light for the horizontal center line of the projected image should be at the same elevation as the body of the projector.
Some projectors (especially short throw) are designed to project upwards from a table or desk, this will not work with the
setup.

If you wish to use DLP projectors, we've done some basic testing of them but have not trained mice with them. The image is slightly out of focus in certain places, but it is not a serious issue. The main issue with DLP projectors is that most do not project straight out of the projector - they project slightly upwards with build in keystone correction. This has two main effects. First, the projector will need to be mounted at a slight angle relative to the top piece of the VR setup (ask Noah if this isn't clear). Second, due to the keystone correct and the angle of mounting, the transformation functions will need to be adjusted appropriately within whatever VR software you are using (if you wish to know exactly where on the screen things are being displayed). The MEX-file `fliptransform_parab_dlp_w_offset` provided under `Software/Virmen/transformations` may work for this purpose.

**Note that the laser cut mount for the projector included in these designs is specifically for the PicoBit.** It will also work with the PicoPro and laser beam pro if you remove the top "alignment" piece and stick the projector to the bottom piece using double sided tape or velcro (the height and geometry works, you will just need to center the image by eye when you stick down the projector). 

If you want to test out a DLP projection with this design, this projector: https://www.amazon.com/dp/B078PC1QS5/ref=sspa_dk_detail_5?psc=1 has the right throw. I'm not sure if it projectes straight out from the projector or slightly upwards. Parts of the projected image will be out of focus due to projecting onto a curved surface, but this might not be so bad for the mouse. If you do try it, let us know how it works!



## Screen Assembly

### Parts list and notes

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/vrRig_v01r1_bpBox_exploded.png)

The parts are listed in the "screen assembly" section of the parts list google doc: https://docs.google.com/spreadsheets/d/1hkXaeRBd7GJPuhIFjeKkEIziQik9mlkZpzjhxGrkA7A/edit?usp=sharing 


#### Mirrors

These should be approximately 1/4" thick and first surface if possible. Non first surface mirrors will result in some double
image "ghosting" but otherwise will work with the design. Acrylic mirrors can be used to save money and time - these can
easily be laser cut. Note that acrylic mirrors typically do not have surfaces as smooth as glass and the projected image may
appear textured as a result.

#### Rails, brackets, and t-nuts

McMaster or Thorlabs or any other 80-20 style rail materials should work. Note that Mcmaster t-nuts / fasteners are not
compatible with thorlabs rails as they have slightly different slot dimensions. Thorlabs t-nuts are compatible with McMaster t-slotted framing. Only the 15" Horizontal rails need to be 1/4-20 end tapped.

#### Acrylic

Any 1/4" acrylic that can be easily laser cut should work fine. It does not need to be matte or black. Opaque acrylic is best to block stray light. Stray light and reflections can also be blocked on any type of acylic using pieces of the black flocked self adhesive paper 

### Part Preparation

These steps will take approximately one hour.

For part preparation you will need the following equipment:

• Power drill

• 1/4-20 thread cutting tap

• Laser cutter with at least 12 x 24" bead, power to cut through 1/4 inch acrylic. (There are online services that will do this cutting for you if you don't have access to a laser cutter. We've been quoted around $400 for cutting these designs, although there may be cheaper options).

Steps:
**1. Laser cut the parts.** The file you should need is https://github.com/HarveyLab/mouseVR/blob/master/Hardware/BackProjection/vrRig_laserCutter_allParts.pdf. (NOTE: for the DLP 3010 projector, use https://github.com/HarveyLab/mouseVR/blob/master/Hardware/BackProjection/vrRig_laserCutter_allParts_DLP_mount.pdf instead.) The scale in the document should be 1:1, inch units, but a small 1x1" scale square is included. Ensure that it is 1" on each side, and scale parts uniformly if necessary. Arrange the parts for cutting. eg, for cutting on two 18 x 32" pieces of acrylic:
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/efficient_parts_layout.PNG)

**2. Tap holes in the laser cut parts.** All round holes in the laser cut parts are either 0.26" through holes, or 0.2-0.21 holes
that should be 1/4-20 tapped. It is safe to assume that if a 1/4-20 screw does not fit through it, it should be tapped. Refer
to the parts drawings if you are unsure. Don’t worry about tapping perfectly perpendicular to the face of the acrylic - we
use a hand drill and eyeball it and that works fine for us. Use a blade to clear out waste acrylic from the parabola-shaped
screen slots in the top and bottom pieces.

**3. Cut screen film to size:** Using a ruler and a pair of scissors, cut out a rectangular piece that is approximately 21.3 x 12
inches.

4. If using McMaster rails or 80-20 that needs to be cut to length, cut 4 pieces 18" in length for the legs, and 2 pieces 15"
in length for the cross braces. Hand tap the ends of 15" pieces with the 1/4-20 thread cutting tap.

**5. If using self-adhesive black flocked paper, cut two ~ 7.5 x 4" pieces.**

### Assembly

Assembly takes approximately one hour.

Refer to the figure below for a visual guide.

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/ScreenAssemblyInstructions.png)

Steps:

**1. Add corner brackets to the bottom piece.** Place the bottom piece (parabolic screen slot, no holes or cutout in center) flat on the table and screw four 1" brackets into it as illustrated using 3/8"-long low profile screws.

**2. Attach sides** using the 1" brackets and 3/8"-long low profile screws. The long flat edges of the sides should be on
the bottom. Stick the cut black-flocked paper / felt pieces to the inside of the sides closest to the screen. Attach four
more 1" brackets as illustrated.

**3. Attach top piece.** Screw into place from the inside using the 1" brackets and 3/8"-long low profile screws.

**4. Assemble the projector mount.** To make sure that you aren’t messing it up, screw the
projector onto the larger of the two pieces using a 3/8"-long 1/4-20 screw through the center hole in the base mount into
the threaded hole on the bottom of the projector. Make sure it is in the orientation such that the fan on the bottom of
the projector is fully visible through the cutout in the acrylic (you may need to flip the acrylic base over). Screw the top
piece of the mount into place as illustrated and then attach the entire thing to the screen assembly using 1/2" (or longer)
1/4-20 screws.

**5. Join the two sides with the end-tapped 15" rails.** Ensure that you are using the right holes and not the holes reserved for
the legs (arranged in vertical pairs 1" apart).

**6. Flip the assembly over. Insert each leg into place**, attaching it to the inside of the assembly using t-nuts or fasteners (see
this note). **Set the height of the screen by adjusting the length that each leg sticks out of the box.**
Attach the right angle brackets to the base of each leg using t-nuts /fasteners.

**7. Repeat for all four legs.** Use a ruler or measuring tape to ensure that all legs are the same height.

**8. If using the acrylic base, attach to the right angle brackets** using 3/8"-long low profile screws. The part of the base that
connects the two sides should be at the back of the assembly, away from the screen.

**9. Flip the assembly upright.** If you are assembling in its final position, bolt the screen assembly down to a breadboard or
air table and insert the two mirrors. Put tape of pieces of black flocked adhesive paper over the openings in the sides to
prevent the mirrors from slipping out. **CRITICAL STEP: the glass first surface mirrors come with a blue protective film
on the coated side. Remove this film and ensure that the coated side ("first surface") is facing towards the screen.** Both
sides will appear reflective but only one side has the reflective coating.
If you plan on moving the assembly, skip this step and insert the mirrors once it is in its final position.

**10. Slide the screen into the parabola-shaped slots.** If it does not fit, there may be waste acrylic stuck in these slots - this
can be easily removed with a box cutter or other blade. cutting a slight V into one of the horizontal edges of the screen
can help with insertion. Once the screen is in place, use binder clips, tape, or glue along the top edge to secure it. Trim away excess screen. We also often apply black tape along the vertical edges of the screen to stick it
to the sides of the box and block out stray light from the projector.

## Ball cup assembly

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/ballCupAssembly_overview.png)

### Parts list and notes

The parts are listed in the "screen assembly" section of the parts list google doc: https://docs.google.com/spreadsheets/d/1hkXaeRBd7GJPuhIFjeKkEIziQik9mlkZpzjhxGrkA7A/edit?usp=sharing 

#### 3D printed ball cup
The ball cup (https://github.com/HarveyLab/mouseVR/blob/master/Hardware/BallCupAssembly/harvey_ballCup_ADNS9800_Vers1_1.ipt) should ideally be printed from a strong plastic that has good chemical resistance and can be easily cleaned. 

We get ours 3D printed by 3Dsystems using the follow specifications:

Process: Selective Laser Sintering (SLS)

Part: https://github.com/HarveyLab/mouseVR/blob/master/Hardware/BallCupAssembly/harvey_ballCup_ADNS9800_Vers1_1.ipt

Material: Glass-filled Nylon (Duraform GF)

Finish: Standard (Coated). Liquid proof entire part with cyanacrylate. Inside of the cup should eb extra smooth. 

Unit price: ~800 USD (prices vary depending on quantity).

These are expensive but work very reliably in our hadns, can be easily cleaned, and have never needed to be replaced even after years of use. 

If you do wish to use a cheaper material and/or printing technology, rigidity - specifically resistance to warping over time - is the most important material charachteristic. My guess is that ABS or a stiff nylon would work. Good chemical resistivity is recommended but not absolutely required. It is good practice to clean the ball cup regularly. Most prosumer or professional FDM, SLA, or SLS 3D printers should provide sufficient resultion for this part. The inside of the ball cup should be as smooth as possible, so lower quality printers may work as well together with some post processing such as sanding or solvent smoothing on the inside of the cup.

See FAQs for more details.


### 3D printed ball cup base

The ball cup base (https://github.com/HarveyLab/mouseVR/blob/master/Hardware/BallCupAssembly/low_profile_noiseAttachment.ipt) does not require as tight tolerances, surface finish, or chemical resistance. We get ours printed in house and probably any material is fine.  

#### Styrofoam ball
The ball cup is designed for an 8" stryrofoam ball. You can buy these on amazon or at most craft stores, but the tolerances will not be great. https://www.wecutfoam.com/ may offer slightly higher quality balls. We lathe our our 8" balls from open cell styrofoam blocks.


## Ball sensor assembly

#### PMW sensors
**For [PMW3360](https://www.tindie.com/products/citizenjoe/pmw3360-motion-sensor/) and [PMW3389](https://www.tindie.com/products/citizenjoe/pmw3389-motion-sensor/) (newer optical mouse sensors)**

These sensors are compatible with [Behavior PCB v2](https://github.com/HMS-RIC/HarveyBehaviorPCB)

1. Solder the [7-pin Molex adaptor](https://www.digikey.com/en/products/detail/molex/0534260710/3263234) to the sensor. **Leave the RST (reset) hole on the sensor unconnected.**

![alt tex](https://github.com/shinkira/mouseVR/blob/master/Guide/PMW_with_Adaptor.png)

2. Use a wirecutter to trim the protruding pins to provide clearance for the installation of the plastic lens.

![alt text](https://github.com/shinkira/mouseVR/blob/master/Guide/PMW_side_view.png)

3. Attach the plastic lens to the sensor board with epoxy (see the above image). Apply epoxy only to the back of the board, where the small pins of the lens assembly protrude through the holes in the board. While the epoxy hardens, place the sensor lens-down on a ﬂat surface to ensure that the lens assembly is seated correctly.

4. Put the sensors into the holes in the ball cup (oriented such that the connector pins ﬁt into the cutaway next to the sensor hole). Make sure that the sensor "slots in" all the way and sits ﬂush with the ball cup. Any failure to make the sensor completely ﬂat/tangent to cup surface will lead to erroneous measurements. 

![alt text](https://github.com/shinkira/mouseVR/blob/master/Guide/Ballcup_with_PMW.png)

5. Use a 2-56 tap to tap the screw holes in the ball cup. Fix the sensor in place with the screws.

6. Connect the [Molex cables](https://www.digikey.com/en/products/detail/molex/0151370706/9759759) to the sensors. (Continue to Step 7 below)

#### ADNS sensors
**For [ADNS9800](https://www.tindie.com/products/citizenjoe/adns-9800-motion-sensor/)**  

Follow these steps to assemble the sensors:

1. Sand down the back of a blue connector so that it ﬁts into the sensor board in the orientation shown in the picture below (notch in blue plastic should face away from center of board).

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/sensor_in_ball_cup.png)

2. Solder the connector onto the board

3. Attach the plastic lens to the sensor board with epoxy. Apply epoxy only to the back of the board, where the small pins of the lens assembly protrude through the holes in the board. While the epoxy hardens, place the sensor lens-down on a ﬂat surface to ensure that the lens assembly is seated correctly.

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/sensor_epoxy-01.png)

4. Use a 2-56 tap to tap the screw holes in the ball cup. Alternately, just use a 2-56 screw to self-tap.

5. Put the sensors into the holes in the ball cup (oriented such that the connector pins ﬁt into the cutaway next to the sensor hole). Make sure that the sensor "slots in" all the way and sits ﬂush with the ball cup. Any failure to make the sensor completely ﬂat/tangent to cup surface will lead to erroneous measurements. Fix in place with the screws.

6. Make ribbon cables to attach the sensors to the board. Do this by cutting lengths of ribbon cable and press fitting the connectors into the ribbon cable using a vice. Secure both sides of the connectors with the included clips. **Make sure that the orientation of the connectors with respect to the colored edge of the cable is the same on both ends.** Double check with a multimeter. 

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/sensor_orientation.png)

7. In the the base, file down a small notch for the ribbon cable to exit along the edge where the cup and base meet. 

8. Orient the ball cup so that one sensor is behind the mouse (it doesn’t really matter which axis is which but it is good to be consistent across rigs).

9. Screw the thorlabs posts and post holders in place on either side of the base, and bolt down the ball using 8-32 screws. Adjust the height of the posts so that the ball cup sits firm and snug on the base. Once you've tested everything, you can put a piece of electrical tape around the seam to make it more air-tight. 

10. Attach the air supply. Screw the duct hose fittings onto each end of the duct hose. Place the rubber stopper into one duct hose fitting and slide the other duct hose fitting onto the attachment point in the base (it should fit tightly). Connect the rubber stopper end of the duct hose to pressurized air using 1/4" tubing. It can be useful to have a pressure gauge & regulator between the wall air and the duct hose, as well as an air filter. You should have at least 4 feet of duct hose in order for the ball to float properly. Add more duct hose to dampen the sound.  

11. Populate the the behavior circuit board. Refer to the behavior PCB section of the guide. At the very least, you need the teensy and the ribbon cable headers. If you want an analog ball signal (as opposed to reading it directly from the teensy) you will also need the RC smoothing circuit (smoothing PWM output of the teensy) and whatever header(s) you will use to read that out (National instruments or BNC).

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/ball_electronics_overview.png).


## Ball sensor software

### PMW sensors 
**PMW 3360 and PMW 3389 are expected to be paired with Teensy 4.0**

1. Download this repo (all files required): https://github.com/HarveyLab/SPI_Mouse_Control/tree/master

2. Install the latest versions of Arduino IDE and Teensyduino.

3. Connect the teensy via USB, and make sure you can identify it in the arduino software. If you are unsure at this step, read documentation and/or use trial code available from Teensyduino ([documentation]( https://www.pjrc.com/teensy/td_usage.html)).

4. Locate the debugMode_PMW folder (SPI_Mouse_Control/Dual-Sensor/PMW/debugMode_PMW) and add the correct firmware file ('PMW3360_SROM.ino' or 'PMW3389_SROM.ino' saved in SPI_Mouse_Control/Dual-Sensor/PMW/SROM) to the same folder.

6. Open the serial monitor (be sure to monitor the COM port your teensy is using), and then verify/upload the code

7. As the teensy reboots, it will print information over the serial port. Verify that the 'Product ID' for both chip 1 and chip 2 is 0x42 (hexadecimal) / 66 (decimal) for PMW3360 and 0x47 (hexadecimal) / 71 (decimal) for PMW3389. See images of expected output, below. If correct product ID is observed, proceed...

8. After booting up, the teensy will continue to live-print various information over the serial port, including the Product ID (should be constant, indicates if connection is valid), a temporally-integrated measure of velocity on pitch, roll, and yaw axes, and indicators of the image quality received by both sensors ('Squal' 1 & 2).

9. Verify by rotating the ball that the correct axis of rotation is inferred from the sensor (errors here usually indicate accidentally swapping Chip 1 & 2), and that the image quality from both sensors is reasonable and of similar level.

10. Once debugging is complete, locate and open the quietOp_PMW.ino file from the dual-sensor folder of repo, and verify/upload the code as before. This code will print out information over the serial port at chip boot-up only, and is preferred for running the circuit when not actively debugging. Once this code has been uploaded and the chip has rebooted the first time, the teensy will automatically re-load this program whenever it is dis-/re-connected from a power source.

Note, the discretization-resolution of the sensors can be controlled by modifying lines 256 & 298 of quietOp_PMW.ino. This involves a trade-off between the dynamic range of the PWM-channel and the smallest-detectable movement, and is set to default at 0x10 (PMW3360 range 0x01 - 0x78, up to 12000cpi with 100cpi step size; PMW3389 range 0x01 - 0x140 up to 16000cpi with 50cpi step size). Be sure any modification is applied to both chips equally, or rotational axes will no longer be properly de-mixed.

Expected output from debugMode_PMW.ino upon startup:

![alt text](https://github.com/shinkira/mouseVR/blob/master/Guide/PMW_initialization.png)

Example expected output of debugMode_PMW.ino for PMW3360:

![alt text](https://github.com/shinkira/mouseVR/blob/master/Guide/PMW3360_running.png)

intP,R,and Y are the pitch, roll, and yaw values. 


### ADNS sensors 

**ADNS 9800 can be paired with Teensy 4.0 or Teensy 3.x (depricated)**

1. Download this repo (all files required): https://github.com/HarveyLab/SPI_Mouse_Control/tree/master

2. Install the Arduino IDE and Teensyduino. If you use Teensy 4.0, you can install and use the latest versions of Arduino IDE and Teensyduino. If you use Teensy 3.x, note that software will not work for all versions of Arduino IDE and Teensyduino (confirmed does not work with latest arduino/teensyduino as of April 2018 - it will compile and run but give erroneous output). An installer for verified working versions  (Arduino 1.8.2, Teensyduino 1.36) is located here: https://github.com/HarveyLab/mouseVR/tree/master/Software/BallSensor. It is highly recommended you use these versions. 

3. Connect the teensy via USB, and make sure you can identify it in the arduino software. If you are unsure at this step, read documentation and/or use trial code available from teensyduino (documentation: https://www.pjrc.com/teensy/td_usage.html)

4. Locate the debugMode.ino file in the repo (in SPI_Mouse_Control/Dual-Sensor/ADNS) and open it. Be sure the 'ADNS9800_SROM' file is in the same folder (i.e. don't change the location of these files from how it is in repo).

5. Open the serial monitor (be sure to monitor the com port your teensy is using), and then verify/upload the code

6. As the teensy reboots, it will print information over the serial port. Verify that the 'Product ID' for both chip 1 and chip 2 is 0x33 (hexadecimal) or 51 (decimal). See images of expected output, below. If it is not, there is a low-level problem with teensy to sensor communication, usually due to incorrect wiring: immediately unplug teensy from USB to prevent sensor burn-out and review entire wiring setup to be sure pin/cable orientation is not accidentally flipped. If correct product ID is observed, proceed...

7. After booting up, the teensy will continue to live-print various information over the serial port, including the Product ID (should be constant, indicates if connection is valid), a temporally-integrated measure of velocity on pitch, roll, and yaw axes, and indicators of the image quality received by both sensors ('Squal' 1 & 2, see ADNS9800 manual included in repo for more information).

8. Verify by rotating the ball that the correct axis of rotation is inferred from the sensor (errors here usually indicate accidentally swapping Chip 1 & 2), and that the image quality from both sensors is reasonable and of similar level.

9. Once debugging is complete, locate and open the quietOp.ino file from the dual-sensor folder of repo, and verify/upload the code as before. This code will print out information over the serial port at chip boot-up only, and is preferred for running the circuit when not actively debugging. Once this code has been uploaded and the chip has rebooted the first time, the teensy will automatically re-load this program whenever it is dis-/re-connected from a power source.

Note, the discretization-resolution of the sensors can be controlled by modifying lines 116 & 121 of quietOp.ino. This involves a trade-off between the dynamic range of the PWM-channel and the smallest-detectable movement, and is set to default at 0x10 (range 0x01 - 0x29). Be sure any modification is applied to both chips equally, or rotational axes will no longer be properly de-mixed.

Expected output from debugMode.ino upon startup:
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/teensy_expected_ouput.PNG)

Example expected output of debugMode.ino for stationary ball:
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/teensy_expected_ouput_running.PNG)

intP,R,and Y are the pitch, roll, and yaw values. 

## Reward system

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/circuit_board_wiring.png?raw=true)


Picture of populated solenoid valve circuit:
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/ee388adf-3a10-4eb0-b3e0-552e36182ac2)



### Lick detection circuit

Use the same  connector (to plug into PCB) and wire as the solenoid valve circuitry, but on the lick spout end, strip both red and black (ground) wires. The red wire will be connected to the lick spout and the black wire should be electrically continuous with the mouse. 

Red wire to lick spout assembly: 
Wrap the stripped end of the red wire around a section of the metal lick spout and secure it in place using either tape, zip tie, and/or heat shrink tubing. Basically it just needs to be continuous with the metal of the lick spout - this can be checked with a multimeter. Ensure that the lick spout is mounted such that it is not connected to ground or electrically continuous with any large metal pieces (like thorlabs posts and clamps, for example). This can be achieved using electrical tape or heat shrink tubing as insulation and/or plastic luer lock connectors that lick spout is screwed in to. For example, you can screw the base of the lick spout into a plastic luer lock connector with barb for reward tubing and clamp the whole thing into thorlabs SWC,RA90 or similar using a 1/4-20 screw. 

Black wire to mouse:
Attach the black wire to whatever is being used to hold the mouse's headplate. I screw it into our headplate holder (happy to share designs if you need that). You just need to be electrically continuous with the mouse with low resistance. 

Test the lick detector:
I do this with my hand: put one finger on the headplate holder and with another on the same hand touch the lick spout. Lick detector circuit output should be high when you are touching it (i.e. connecting spout and ground), low otherwise.

Note: do not assume that if you are electrically continuous with the headplate then you are continuous with the mouse. This might not be the case depending on your surgical prep. If you are getting poor lick detection through the mouse but the lick detector is working perfectly when you test it (i.e. with your hand as described above) then it is possible that the mouse is not properly grounded. This can be easily remedied by placing a drop of ultrasound gel touching both the headplate and the mouse's skin (I sometimes would put it behind one ear) - that has always been sufficient in my hands to bridge the mouse to ground. 



## Behavior PCB Overview

**The behavior PCB has been upgraded to [Version 2](https://github.com/HMS-RIC/HarveyBehaviorPCB)**. While similar to Version 1, Version 2 is compatible with Teeensy 4.0 and includes additional functions.

The behavior PCB has the circutry to monitor two lick inputs (for example if using lick left / lick right setup), control two independent solenoid valves for dispensing liquid reward, and interface with ball movement sensors. 

You can read sensors and control valves using **(a)** the teensy directly (with serial communication) **(b)** a USB-3001 NIDAQ plugged into the board **(c)** some combination of the teensy and the NIDAQ. 

**Version 2**
![alt text](https://github.com/shinkira/HarveyBehaviorPCB/blob/master/HarveyBehaviorPCB_v2_3DModel.jpg)
![alt text](https://github.com/shinkira/HarveyBehaviorPCB/blob/master/PCB_v2_KiCAD/img/BehaviorPCB_v2_populated.jpg)

**Version 1** (Deprecated)
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/HarveyBehaviorPCB_v1.2.png)
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/populated_behavior_PCB.jpg)


Note in the photo of the populated board the MOSFETs in the reward circuit are not pictured. Refer to "Reward Circuit" for an image of the populated circuit on that section of the board. https://github.com/HarveyLab/mouseVR/tree/master/BehaviorElectronics contains complete schematics and bill of materials, so it is best to use those as a guide when populating your board. 


**Options for reading in licks:**
1. Read in licks as an analog signal exactly like the ball signal. Note you may miss licks this way if your sampling rate is low.
2. Attach the lick output to a counter channel on your DAQ & read the counter channel (not sure if these boards are capable of having counter channels or not) http://www.ni.com/academic/students/learn-daq/digital/ 
3. Set up a counter on the teensy and read directly from the teensy using serial communication

**Options for controlling reward valve:**
1. Control using the DAQ analog output (User 3 and/or 4). Set duration of valve opening in your matlab code (e.e. write a giveReward function). You wil need to install jumpers (see fig below) if you haven't already. 
2. Control using teensy triggered by DAQ (User 3 and/or 4, always accesible to teensy, even w/out jumpers present)
3. Control using teensy triggered by serial communication

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/behaviorPCB_jumpers.png)

NIDAQ connections

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/behaviorPCB_NIDAQConnections.png)

### Getting started

The parts are listed in the "behavior electronics" section of the parts list google doc: https://docs.google.com/spreadsheets/d/1hkXaeRBd7GJPuhIFjeKkEIziQik9mlkZpzjhxGrkA7A/edit?usp=sharing 

The files to get the circuit board printed are in https://github.com/HarveyLab/mouseVR/tree/master/BehaviorElectronics/HarveyBehaviorPCB_v1.2 
We use 4pcb.com 

The top will be screen printed with parts codes that you can look up in the bill of materials here: 
https://github.com/HarveyLab/mouseVR/blob/master/BehaviorElectronics/HarveyBehaviorPCB_v1.2_BOM.xlsx

Also refer to the board schematic here:
https://github.com/HarveyLab/mouseVR/blob/master/BehaviorElectronics/HarveyBehaviorPCB_v1.2.pdf

Final board should look like this:


# VR rig details

Mouse positioning relative to the screen:
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/mouse_positioning_on_the_ball.png)
You can place the mouse further back to allow for more room for equipment and this seems to be fine for behavior. 

Very approximate visual field coverage with the placement of the mouse as above:
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/approximate_visual_field_coverage.png)



# Software

## ViRMEn transformation functions
We use the ViRMEn virtual reality engine: https://pni.princeton.edu/pni-software-tools/virmen.  

https://github.com/HarveyLab/mouseVR/blob/master/Software/Virmen/transformations/fliptransformCyl_NPparab.m Is an example of a ViRMEn transformation function that works reasonably well with this setup. It isn't the most accurate but looks fairly good to me and I've had good mouse behavior. Some people have approximated the parabola as a cylinder which can speed things up. If you're doing something that requires knowing exactly where in the visual field something is presented, don't rely on this function.

For the DLP lightcrafter build contributed by Ethan, there is are transformations functions in https://github.com/HarveyLab/mouseVR/tree/master/Software/Virmen/transformations.

# Headfixation

Head-fixation harware is somewhat is independent of the VR setup, but here are some designs for headplates and headplate holders developed in the Harvey lab for our experiments.  

Design files are in: https://github.com/HarveyLab/mouseVR/tree/master/Hardware/HeadplatesAndHolders  

## Headplates

All of the following headplates are designed to be cut from ~0.04" titanium. Waterjet cutting arguably produces the smoothest edges but the easiset and fastest service for getting them made is sendcutsend.com. 100 headplates will cost ~$130 and each can be reused many times. For ordering, upload a dxf file (either one in this repository, or export from inventor by right-clicking the face of the part and selecting "export face as...") and select Grade 5 Titanium (.040") as the material. 

All units are by default inches.

### headplate_largerCenter
This is the "standard" headplate using the harvey lab. It is called "larger center" because the opening int he middle has been expanded from older versions to accomodate larger cranial windows. 
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/461ed404-1e26-4214-b0c4-d952b82e70b0)

### headplate_largerCenter_frontcutout
Modified from headplate_largerCenter to have the front cut out. This was designed for CA1 imaging to allow the headplate to be placed much closer to the skull to give enough working distance for the objective to image through a cannula. It alos provides a bit more flexibility to in terms of headplate positioning relative to the window.
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/d6efa4ae-15b0-467a-ae10-0ede92ac6007)

### headplate_largerCenter_frontsidecutout
Modified from headplate_largerCenter to have the front cut and side cutout. One clamping fork was removed for better positioning during ca1 surgery. In conjuction with the two-sided headplate holder, fixation is very stable (e.g. for 2p imaging, no vibrations or motion outside of normal brain motion) and highly reproducible across days (~on the order of a few tens of microns).
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/5a26d8d4-9561-42a6-ad36-10ffe2f3c727)

### harveyLabWideHeadplate_1p4in
Version with longer forks coming out side
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/b10e4879-3918-4186-93f0-9c8a90087fff)


## Headplate holders

### twoSidedHeadplateHolder_NP_v02
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/ba1001da-8af1-42fa-b5ca-3df845863362)
Designed for reproducible mounting across days, especially for pitch, roll, and yaw, which cannot be compensated for by XYZ stages.  (translation reproducibility is on the order of a few tens of microns which can be easily compensated for using the stage). Note some of the holes are for clamping screws, some are for alignment pins or set screws (more on that below).

Machined from aluminium. Small holes should be 4-40 tapped, larger holes can optionally be 1/4-20 tapped for affixing lick spout hardware. No fillet or chamfer on the edges so make sure to ask the machinist to deburr the edges. Note the fillets where the two arms become thinner are uncessesary and could just as well be steps or whatever is easier to machine. 

For imaging setups it should be milled from aluminium. For training rigs where micron-level stability is not needed, 3d printed or laser cut plastic is ok. 1/8 inch acrylic will flex and vibrate a bit but I have still used it to train mice and it was fine. 

I recommend using these screws for attaching the headplate:
https://www.mcmaster.com/98164A429/
Shorter screws (1/8") will work too but will wear out the threads in the headplate holder faster. 

These set screws should be used for alignment:
https://www.mcmaster.com/92311A105/

Here is the configuration I of screws I have used:
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/e9d791ed-c585-43de-ac46-d19a23f3452c)

Set screws in the alignment holes can be used to provide 3 points of contact between the edges of the headplate and the holder, thus providing reproducible mounting to within a few tens of microns, and constraining rotational degrees of freedom. The arrow indicates the direction of force I apply to the headplate while clamping it in place. I usually clamp in place with screws in clamping holes 3 and 1, or 2 and 3, or Clamping hole 2 and Alignment hole 3, but you can experiment with what works well for you. The unlabelled holes can be used to store screws - for example if clamping with holes 2 and 3 - one of the screws will need to be moved into place one the headplate is in position. 

It is designed to be mounted to this kinematic plate from thorlabs: https://www.thorlabs.com/thorproduct.cfm?partnumber=KB3X3

I mount the kinematic plate and headplate holder to this https://www.thorlabs.com/thorproduct.cfm?partnumber=C1515 for use with a 1.5 inch post for imaging.

I used these damped posts but they are probably overkill, any 1.5" post would do. https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=170. 

The kinematic mount allows for the headplate holder to be removed for cleaning the ball and cup and put back in place in the exact same position. 

The mounting can be easily customized but having it on a kinematic base allows you to maintain reproducible positioning while also being able to remove the headplate holder to clean and remove the ball and access ball cup. I suggest while the mouse is on there to use a single long screw through the center hole of the kinematic base into C1515 bracket (finger-tight) to ensure everything is fixed in place, since the magnets of the kinematic base are not super strong.  

![image](https://github.com/HarveyLab/mouseVR/assets/16245463/8c003e40-7559-49cb-9e46-0239323f9744)

### twoSidedHeadplateHolder_wide_sendcutsend and twoSidedHeadplateHolder_v3_scs.dxf

A version designed to be cut by sendcutsend.com from a flat sheet of stainless steel. Each one costs about $80, which is cheaper than getting the aluminum one machined. Another advantage is that the 4-40 hole threads are prone to wear out over time and this happens quite a lot more slowly with steel. The disadvantage is that the material is a bit thicker and send-cut-sent dimensional accuracy does not quite match CNC machining. To place an order: Upload twoSidedHeadplateHolder_v3_scs.dxf and choose material Stainless Steel (304 series) (.125"). Select 4-40 tapping for smaller holes and 1/4-20 tapping for larger holes. It is possible that thinner versions of stainless steel would work as well but we have not tested those.  This design has more holes than the other headplate holder in order to accomodate ever wider headplate designs (such as harveylabwideheadplate_1p4in.dxf). However it should be backwards compatible with all the headplates listed here. 
![image](https://github.com/HarveyLab/mouseVR/assets/16245463/c0193c0c-7c81-4289-aa3b-a92cc520f9f2)



# FAQs

## What are the computer hardware requirements?
The projector is driven simply as another monitor, so this really depends on the VR software that you are using and the complexity of the worlds that you need to render. Consult the documentation for whatever visual stimulus or VR software you are using: Our computers use GTX 1060 cards (https://www.amazon.com/GR8-II-T044Z-Desktop-i5-7400-GeForce/dp/B06WLHT15C is one model we have used successfully) but this is overkill for most of the worlds we render. Integrated graphics are a bad idea and will likely result in <60FPS. For ViRMEn we suggest at least a 3.00GHz CPU and 8GB of RAM. 

## What are the pros and cons of using this setup over multiple monitors?
The pros (+) and cons (-) of multiple monitors:

(+) slightly better availability and choice of different screen types and resolutions. may be better if you care about precise visual stimulus timing as gaming monitors are available w/ high frame rates and low input lag.

(+) Potential to be more compact (although this design is already very compact).

(-) Requires either better graphics card to drive 3+ monitors or additional hardware such as "triple head-to-go" (which may complicate transformation functions and introduce lag - you'd need to test). 

(-) Bezels: not a big deal for navigation stuff, but does disrupt things like whole-field optic flow, visual RF mapping in same experiment, etc. May be less immersive.

(-) May be harder to correct for illumination across visual field as that can vary w/ angle that the mouse is seeing the screen from.

(-) Less flexible configuration - with our design you can cut holes, notches, slots into any part of the screen etc to fit optics, microscopes, cameras, etc. You can easily resize the screen and adjust design. We have found this to be very useful.

The projector also has the advantage that it's easy to apply filters to the output of the projector. For example: dimming the brightness uniformly with an ND filter, or applying a short-pass filter to attenuate hard-to-light-block red wavelengths (that the mouse can't see anyway) during imaging experiments. 

We have used both a 3 monitor setup and this design and have not found a difference in terms of behavior.

## Many of these parts are expensive for our lab - are there cheaper alternatives?

In many cases there are cheaper alternatives. Some of these are listed in the parts list. 

For example:

Ball cup: May be printed with cheaper materials / quality than the quote listed. We have not explored this. See ball cup section in this guide for more details.

Projector: Low resolution DLP mini projectors are very cheap! Using different projectors may require some small modifications to the hardware. See projector section for more details. 

Thorlabs: most of these parts can be replaced with equivalents from McMaster, 80-20, amazon, etc. 

First surface mirrors: First surface mirrored acrylic is also available for much cheaper and can be laser cut. There will be a slight textured look to the projected image (due to surface not being as flat) but it is subtle and I'm pretty sure it wouldn't influence behavior. 

Acrylic: There are cheaper sources than Canal Plastics for the acrylic. It does not need to have high thickness tolerances and it does not need to be black - any color will work. 

## I want to use metric screws / hardware - are these deisgns compatible?

We haven't tested this but it should be! Use M6 screws / taps in place of 1/4-20. Please let us know if this works! Everything was desinged in inches, so if units appear undefined for a particular drawing, please assume inches.  

## How do I order the behavior PCBs

We use 4pcb.com $33 per board special: https://www.4pcb.com/pcb-prototype-2-4-layer-boards-specials.html 

1. Select 2 layer, 5 day turn

2. Sign into your account (or create one)

3. Step 1 of 4: upload files. Upload the .zip file in https://github.com/HarveyLab/mouseVR/tree/master/BehaviorElectronics

4. Part#: HarveyBehaviorPCB_v1
Rev#: 1.2
X dim(in): 8
Y dim(in) 4.95
Soldermask: both side
silkscreen: top side
layers: 2
Quantity: 10 (or whatever you want > 3)
Multiple Images: No
Ship methoid: your choice
ITAR: No

5. Enter shipping and billing info 

6. Comments: STUDENT

## How do you calibrate the ball sensors?

First, make sure that the sensors are oriented the same way as we have them and offset by 90 degrees in yaw. Make sure that the sensor sits ﬂush with the ball cup. Any failure to make the sensor completely ﬂat/tangent to cup surface will lead to erroneous measurements. Note that the part of the sensor chip with the ribbon cable socket should be towards the top of the ball! See the ball cup assembly section of the guide for more info. 

The teensy outputs velocity signals for pitch, roll, and yaw in arbitrary units. The magnitude of these readings can vary depending on a bunch of different things including (I think) the distance of the ball from the sensors. It is recommended that you somewhat regularly check calibration since the styrofoam balls naturally wear down over time and this change in size can influence the readings. It shouldn't drift by a huge amount, but better to be safe.

Calibration is very simple and we do it regularly. Hook everything up, set up your VR software and movement function, and rotate the ball by a set amount in pitch, roll, or yaw. Pitch corresponds to forward running and we typically use roll for either lateral or view angle changes. After rotating by a set amount (for example, 2 ball rotations), note the distance travelled in the virtual environment (either in view angle degrees or translational movement). Knowing the circumference of the ball (~63.8 cm), you can set gain(s) in your vr software appropriately such that you have a certain number of vr_units per cm. 

## What factors are important for mouse positioning relative to the ball and the screen?

Note that positioning on the ball and relative to the screen can be thought of as pretty much independent of each other. If you want to change the positioning relative to the screen i'd recommend moving the ball cup assembly, and not repositining the mouse relative to the ball. More about the factors that go into the positioning:

1. Positioning on the ball. This positioning has been optimized in the early days of Chris's lab for good mouse behavior and ball control. For tasks in which the mouse needs to turn/steer, we typically use roll for side-to-side translation or view angle and pitch for forward movement. If the mouse is sitting towards the top of the ball it will often result in the mouse spinning the ball mostly in yaw (it may be hard to imagine this but once you have a mouse on the ball you'll see what I mean). Placing the mouse further back on the ball increases rotation in pitch and encourages forward running. I believe steering is also aided by this slightly further-back position. If your task requires turning or steering I'd recommend starting with this position and adjusting from there. If you are just using a linear track & a yaw/roll blocker then the ball becomes a treadmill and positioning is not quite as important. 

2. Positioning relative to the screen. One factor to consider here is the focal distance of the mouse's vision. Everything beyond about 5 inches is in focus for the mouse (reference?). I'm not sure how detrimental it would be to have parts of the screen closer than this and thus out of focus. We discussed increasing the peripheral coverage when designing the VR and decided that it wasn't worth it for what we use it for. 210 deg of visual coverage is fine for behavior and we get nice place cells with it as well.
If you decide to move the ball closer to the screen, you may want to create a small cutout in the bottom of the screen so that the ball doesn't hit into it, or just raise the screen by a bit. 



# Troubleshooting

# Appendix

## Part drawings / dimensions:


### Screen assembly
![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/vrRig_screen_mainDimensions.png)

### Modified screen assembly with Texas Instruments DLP3010 
From Oleksandr Radomskyi, Santiagio Rompani Group, EMBL, Rome
![alt_text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/mousevr_dlp_1.png)
![alt_text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/mousevr_dlp_3.png)


### Ball cup

### Ball cup base

### Enclosure

### Slide out

low profile drawer slides mounted with laser cut acrylic for pulling the ball cup assembly away from the screen in order to more easily mount the mouse and access the ball cup for cleaning. Slides should not be used with ephys or imaging due to instability and vibrations. 

![alt text](https://github.com/HarveyLab/mouseVR/blob/master/Guide/slideOut_schematic.png)






Contributors: Noah Pettit, Matthias Minderer, Selmaan Chettih, Charlotte Arlt, Jim Bohnslav, Pavel Gorelick, Ofer Mazor, Christopher Harvey.

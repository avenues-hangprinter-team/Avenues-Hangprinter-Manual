# Avenues Hangprinter Usage Manual

### Topics:
  - [Power](#power)
  - [Octopi](#octopi)
  - [G-code](#g-code)
  - [What to print?](#what-to-print)
  - [Printing](#printing)




<a name="Power"></a>
## Power

Before using the printer, make sure it is connected to power and that the voltage regulator is outputting 5V. That is the correct amount of voltage used to power the Raspberry Pi and the Smart Steppers. If the value shown is bigger, immediately shut down the system.

<a name="Octopi"></a>
## Octopi

To send and receive commands from the printer, we are using a Raspberry Pi 3 B+ connected to the Arduino, and a wireless platform called Octopi. To login, access [octopi-2.local](http://octopi-2.local) and use the following logins:

Username: `user`  
Password: `Avenues1`
_\*for regular use_

Username: `admin`  
Password: `avenuesadmin`
_\*only when making configuration changes_

>

  - If the website is not loading, first make sure you are connected to the network `AVEWIFI_-1`. If it still refuses to load, try going to [octopi.local](octopi.local) or [octopi-3.local](octopi-3.local).

<a name="G-code"></a>
## G-code

3D printers use a specific set of commands to print and communicate, they are called G-code. After having an established connection with Octopi, you will be able to look at the overall status of the Hangprinter, which includes things such as hotend temperature, fan speed etc.

If needed to calibrate or test any sort of movement, some G-code commands are essential to know. To send them to the printer, access the `Terminal` tab.

The general structure of G-code is essentialy composed of three parts. The following is an example G-code command.

> G6 A60 B60 C60 D50 F1000

If executed, this command will make motor A, B and C move 60mm, and motor D move 50mm. All of them will do that in a _feed rate_ of 1000.

Simplifying the three main parts mentioned before, they can be seen as **_what_**, **_who_** and **_how_**.

- As you can probably tell, the **_what_** is represented by the `G` followed by a number. The numbers chosen represent different actions a printer can perform.

- The **_who_** can be in a diverse range of options. In this example, it was the motors `A`, `B`, `C` and `D`. Notice how each of them were followed by a number. In this case, that number represented the amount of movement in millimeters.

- Finally, the **_how_**. Depending on the `G`command chosen, it can mean a variety of different things. In the example shown, it meant the feedrate of the movement being executed, represented by the letter `F`.

---

To assist you in your first "conversations" with the Hangprinter, here are some of the most important G-code commands to know:

- `G0, G1` - _Moves the gantry_

  > G1 Z200 F1000
  > //moves the gantry 200mm along the Z axis at a feed rate of 1000

- `M104` - _Sets the extruder temperature_

  > M104 S200
  > //sets the temperature of the extruder to 200°C

- `G95` - _Sets the printer to torque mode, which is further explained in the "Calibration" section_

  > G95 A35 B35 C35 D35
  > //puts all axes into torque mode at a level of 35/255

- `G92` - _Sets an origin position to the printer_
- `M114` - _Receive the current position of the gantry. This is useful during the calibration process._

<a name="Wtprint"></a>
## What to print?
With any 3D printer, the first step to printing is either designing or downloading a 3D object that you would like to see in the real world.

If you don't really want to design your own prints, [Thingiverse](https://www.thingiverse.com/) is an excellent place to find the awesome things made by the 3D printing community.
But if you want to go and design your own creation, we recommend two pieces of Software. For beginners, [tinkercad](https://www.tinkercad.com/), a simple to use intuitive online editor. For individuals with previous experience in CAD, Fusion 360 is an amazing option.

For a first print, a cube or a 3D Benchy are usually sufficient, as you will be able to see the issues with the printer easily.

After you've chosen what to print, there is still one more step to take: slicing. For that, programs as Cura and Slic3r are recommended.
Slicing serves to transform a 3D object into a series of G-code commands for a specific printer.

In the slicer settings, you should set a profile for the Hangprinter. The settings we used are:

- Layer Height: 3mm
- Nozzle size: 1mm
- Temperature: 200
- Speed: 150mm/s

Start G-Code:

>G1 X0 Y0 Z0 F3000
G1 Z15.0 F3000 ; Move the platform down 15mm
>G92 E0
>G1 F200 E3
G92 E0
G92 E0
G1 F1500 E-6.5 ; Prime the extruder
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up to prevent scratching of Heat Bed
M107
G0 F3600 X17.75 Y-10 Z0.3

<a name="Printing"></a>
## Printing

It is finally time to print. After having a G-code file correctly sliced for the printer, upload it to Octopi in the `Load File` tab.
Before starting your print, there are some necessary "pre-flight" checks:

- _Hot end temperature:_
  If you are printing PLA, the recommended is around 185°C to 205°C.
  >
- _Z height_:
  The correct distance from the nozzle to the print surface is essential for a good print. To test it, slide a piece of paper between them. There should be some drag, but not enough to create marks on the paper's surface.
  >
- _Levelling_:
  If the print surface is not correctly levelled, the chances of a bad print are certain. Always make sure everything is calibrated and aligned properly.
  >
- _Wire Tightness_: 
  Ensure that all of the wires are properly taut. If any lines are not taut follow the following steps:
  - *Are both lines loose?*
    - **Yes**
    1. Check the power of the system and see if the stepper motors are on.
    1. Try setting the Smart Steppers into torque mode using `G95 A35 B35 C35 D35`(Hold mover while doing this and reset to lock mode after, using `G95 A0 B0 C0 D0`).
    >
    - **No**
    1. Make sure that the anchors are parallel to the corresponding beam on the mover. Re-calibrate as needed.




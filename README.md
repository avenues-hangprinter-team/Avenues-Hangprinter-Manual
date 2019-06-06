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

AMA 悬挂式打印机引导
目录：
Avenues Hangprinter Usage Manual  目录
Power  电源
Octopi  Octopi
G-code  G-code
What to print?  打印什么？
Printing  打印
电源：
在使用打印机前，请确保稳压器的输出电势是5V.这是Raspberry Pi和Smart Stepper的正确使用电压。如果电势大于5V，请立即切断整个系统的电源。
Octopi：
若要发送或接收打印机指令，我们通过一个叫做Octopi的无线平台与Raspberry Pi 3 B+和Arduino连接。若要登陆，请访问http://octopi-2.local/，下面是用户名和密码：
用户名（日常使用）：user
密码：Avenues1
用户名（仅用于更改配置）：admin
密码：avenuesadmin
注：如果网站不加载，请确定你的网络是AVEWIFI_-1，若网络还是不行，请尝试以下链接：octopi.local 或 octopi-3.local.
   G-code：
3D打印机需要一套特殊的指令系统来收发信息并打印物体。这套指令叫做G-code。当我们成功与Octopi连接之后，你可以查看打印机的整体状态，包括风扇转速和喷头温度等等。
如果需要校准或测试，某些G-code指令对于我们来说十分重要。若要发送指令，请点击Terminal按键。
G-code由三个不可忽视的部分组成，下面我会po出一个例子：
G6 A60 B60 C60 D50 F1000
如果电脑执行了上述指令，则会使ABC马达移动60毫米，使D马达移动50毫米， 挤出机的挤出速度是1000.
简单来说，前面提到的这三个部分可以用 what who和how来阐述
       -我们可以说，what 是 G+数字，这个数字决定了打印机如何移动。
       -而who，则代表着许多选择。ABCD代表着顶板的四个驱动电机，而字母后的数字代表着移动多少毫米。
       -how，则由G指令决定，这可以代表很多东西，比如说进料速率，由字母F表示。

在开始与你的悬挂式打印机甜蜜对话前，我们需要知道一些最重要的G-code
    -G0 G1：移动三角结构   
    G1 Z200 F1000 //在Z轴上移动三角结构200毫米，进料速率1000
   -M104：设置挤出机温度
       M104 S200 //将挤出机设置为200摄氏度
   -G95:将打印机设置为力矩模式（torque mode），我们在“校准”这一章节中会向您介绍
     G95 A35 B35 C35 D35 //将所有轴设置为35/255级的力矩模式。
   -G92：设置打印机原点
   -M114：显示三角结构的当前位置。这个功能在校准时很有用。

打印什么玩意？
一般来说，使用打印机的第一步要么是自己设计一个物体模型，要么从网上下载一个模型来打印。你可以打印生活中比较常见的东西。

如果你懒得设计一个东西打印，Thingiverse是一个绝佳的网站。你可以在这个网站上免费下载海量3D模型，这些模型由3D打印社区制作并上传。如果你想体验设计的乐趣，我们推荐两款软件。对于初学者，我们力荐tinkercad，你可以在线学习如何设计并尝试设计简单的小物件。而在设计（CAD）方面有着十足经验的人，我们更推荐Fusion360，满足一切专业且精确的要求。

第一次打印，你可以打印一个小方块或一个3D 束*（原文：3D benchy）。通过打印这种简单的小物件，你很容易就能找出打印机潜在的问题。

当你选择完毕你决定打印的物件之后，你需要将打印物导入切片软件（sliceing software）我们在切片软件方面比较推荐Cura和Slic3r。切片工艺能将3D物体变成一串串G-code。

在切片软件设置里，你需要设置一系列数据。这些数据分别是
Layer Height 层厚: 3mm
Nozzle size 喷嘴大小: 1mm
Temperature 温度: 200
Speed 打印速度: 150mm/s
开始的G-code：
G1 X0 Y0 Z0 F3000 G1 Z15.0 F3000 ; Move the platform down 15mm G92 E0 G1 F200 E3 G92 E0 G92 E0 G1 F1500 E-6.5 ; Prime the extruder G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line G92 E0 ; Reset Extruder G1 Z2.0 F3000 ; Move Z Axis up to prevent scratching of Heat Bed M107 G0 F3600 X17.75 Y-10 Z0.3
打印：
是时候打印了呢。当G-code设置完成后，进入Octopi并单击 Load file。在开始打印前，请检查以下事项：
-加热头的温度：如果你要打印PLA，温度最好位于185摄氏度到205摄氏度之间。
-Z轴高度：喷头和地板的距离是决定打印质量的关键因素之一。最佳距离判断方法：将一张纸放在喷嘴下地板上，若能感觉到喷嘴轻微刮纸却又能让纸张有阻尼感的移动，则是最佳距离。
-调平（leveling）：优秀的调平也是高打印质量的一个关键因素。确保每一次打印都经过校准。
-线缆捆绑：确保所有鱼线紧的一匹，如果鱼线有点松，请参照下列步骤：
       ----所有的线都是松的？
                          a。是的
                          b。检查系统以确保所有步进电机工作。
                         c。设置Smart Steppers使其进入力矩模式，并输入G95 A35 B35 C35 D35(握                                                                                                   住三角形可移动部分，重设锁定模式，输入G95 A0 B0 C0 D0）
                          d。No
                          e。确保所有的固定锚平行于三角形的边，如果还不行就重新校准



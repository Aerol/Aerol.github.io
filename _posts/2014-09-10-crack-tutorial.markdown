---
layout: post
title:  "Crack tutorial"
date:   2014-09-10 14:10:17
tags:   tutorial crack software
---
To start you're going to need OllyDbg, you can obtain it from
[http://home.t-online.de/home/Ollydbg/](http://home.t-online.de/home/Ollydbg/)

After grabbing Olly and unzipping the archive, copy this ollydbg.ini
[insert link to my ollydbg.ini] into the Olly directory.
Run Olly and you should be presented with an error message about the
UDD directory not being present.
Click OK and goto "Options->Appearance" and setup the UDD and Plugins
directory for your system. (sub-directories in the Olly folder works fine)
Click OK and close Olly.

Download the OllyDump plugin from [tuts4you.com](https://tuts4you.com/download.php?view.88).

Place the dll in the plugins sub-directory created earlier.
Start Olly and you should now have a new sub-menu under Plugins called
OllyDump.

In Olly, hit F3 or goto File->Open (or the folder icon in the toolbar)
and open the iisc.exe from the 2nd Speech Center program directory.

Olly will open and analyze the exe and give you the disassembled view
along with lots of other info about the executable in memory.
You should have a screen similar to the screenshot below.

![Screenshot 1](/images/ssc_tut1.png)

Press F9 to run the program in Olly, you will be presented with the
program's register nag screen where you can insert registration info
and unlock the program.
Type in whatever name you want and whatever registration code you want
and click "Unlock Your Copy".
You'll get an error message as in the screenshot below.

![Screenshot 2](/images/ssc_tut2.png)

Take note of the error message, click OK and head back to Olly.
Right-click in the disassembly window and select "Search for->All
reference text strings"
In the new window that comes up right-click and select "Search for
text". Type in the error message in the input field and select "Entire
scope".
Part of the error message will work.

![Screenshot 3](/images/ssc_tut3.png)
![Screenshot 4](/images/ssc_tut4.png)

When the search funtion finds the string, press enter and you'll be
taken back to the main disassembly window of Olly and where the string
is will be highlighted and a red line will be next to it.
Follow the red line up and you'll see it points to 3 jump
instructions. (2 JEs and 1 JNZ)

![Screenshot 5](/images/ssc_tut5.png)

Set a breakpoint on each jump instruction. (click on the instruction and hit F2)

![Screenshot 6](/images/ssc_tut6.png)

Restart the iisc.exe process by hitting Ctrl-F2 or by clicking the
double left arrow icon in the toolbar.
Now you should be able to run the process, input some false info and
step-over the checking process.
As soon as you click "Unlock Your Copy", Olly will stop execution at
the first breakpoint set.
In the smaller window below the disassembly window you can see that
the jump is not taken, so you can proceed to step through the process.

![Screenshot 7](/images/ssc_tut7.png)

Since there's another breakpoint close by, you can just hit F9; again
you'll see that the jump is not taken.
Now, between the second breakpoint and the third there are a lot more
instructions, two of which are call instructions.

Pressing F9 again will stop us at the third breakpoint and in the
lower window, you can see that the jump is going to be taken!

![Screenshot 8](/images/ssc_tut8.png)

Look over in the registers window and double click the Z flag. (should
set it to 1, meaning the Z(ero) flag is true.)

![Screenshot 9](/images/ssc_tut9.png)

Now that lower window will say that the jump is not going to be
taken. (JNZ means Jump if Not Zero, by setting the Zero flag to true,
this jump will never be taken)
Press F9 and you should be presented with a congratulations message
from the iisc process.

![Screenshot 10](/images/ssc_tut10.png)

Click OK and head back to Olly.
Select the JNZ instruction we bypassed and press the space bar.
Now, there are a few things you can do but for now you can just
replace what's in the input field with "nop". (nop stands for No
OPeration)
Make sure Fill with NOP's is selected and press enter.
The JNZ instruction will be replaced with two nop instructions.

![Screenshot 11](/images/ssc_tut11.png)
![Screenshot 12](/images/ssc_tut12.png)

Now in Olly, go to "Plugins->OllyDump->Dump debugged process".
In the OllyDump window, uncheck "Rebuild Import" and select "Method2:
Search DLL & API name string in dumped file".
"Fix Raw Size & Offset of Dump Image" should already be checked, but
if it's not, make sure it's checked.

![Screenshot 13](/images/ssc_tut13.png)

Click the "Dump" button and save to "crack.exe" or whatever you want
really.

And you're done.
Congratulations on creating your first (presumably) crack.

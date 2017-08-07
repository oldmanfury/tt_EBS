# tt_EBS
terminal tedium Euclidean Beat Slicer
Euclidean Beat Slicer basic installation instructions. 

You don’t need an OLED for this to work.  OLED options may be omitted

Copy tt-EBS.pd and ttplot.py to your terminal tedium patch folder.     

Copy tt-EBS.pd to your PC or Mac patch folder. 

ttplot.py is the OLED script.

in pdpd (the batch file that runs puredata - I attached my version for reference, but you should use your original) change the puredata execute command to 

sudo /home/pi/pd-0.46-7/bin/pd -nogui -rt /home/pi/pdpatch/tt-EBS.pd |& python /home/pi/pdpatch/ttplot.py

making sure the paths are right for your installation. 

In ttplot.py - you need to point font2 at a font installed on your tt - check usr/share/fonts/truetype/ and select one that you have.  I used Roboto-Bold.ttf, but not everyone has that.  The line you need to edit is :
font2 = ImageFont.truetype("/usr/share/fonts/truetype/roboto/Roboto-Bold.ttf",14)


Now you need to install your loop files on the tt and on your pc.  Once they're in a folder called loops you need to create a text file list of the files.  You can do this with something like 
"dir/w > list.txt" on your PC or with  "ls > list.txt"  I can't remember the exact syntax.  You want a text file with each filename (no path) on each line.  Add rec.wav to the top of the list (this is your recording-on-the-fly file).  Once your list.txt file is done for the tt or your PC, put it in the folder.  It should look like:

  rec.wav

  DC_BLINE04_170_C.WAV

  DC_BLINE09_175_C.WAV

  DC_BLINE20_170_C.WAV

  etc.

Copy one of your WAV files (or AIFF) and rename it rec.wav so that PD can find it when you run the patch.  This step might not be necessary, but it isn’t hard.

IMPORTANT ANNOYING NOTE - puredata hates spaces - replace them all with underlines or delete them.  No spaces in folder names or filenames!

Now open tt-EBS.pd on your PC and double-click initialize.  The path to your tt loop folder is in a message box (loops/). 

In a tt-terminal window kill puredata with 
"sudo killall -9 pd" 
then execute pdpd again with "sudo ./pdpd"

If all went well it might be working.  To figure out what tt-EBS is doing, use the PC version - you can really see what each knob does with all the array plots.

The six knobs are:

1) number of beats per measure
2) number of effects hits per measure (Euclidean algo spreads them out)
3) offset of the effects relative to each other
4) randomness of playback - at zero it plays all slices sequentially - turn it up slowly and add some randomness
5) number of stutter beats
6) effects selector.  This is a 0-15 binary selector.  Each bit turns on a Euclidean effect pattern
0     0           0      1
rest reverse pitch stutter
turn it to 15 and you get them all on (1111).  Half-way up you get (0111) everything but rests.

Digital inputs
D1 - triggers each step.  Loop needs to be playing, and it does some unintended things right now.  Will be useful for syncing playback with your synth.  Right now - kind of irritating.
D2 - start/stop once you arm for recording
D3 - re-calculate randomness.  Can also change knob one up and down to re-do the random selection.

Help?  Email me at logo64@gmail.com



OLED references:

Configuring i2c
https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c


Adafruit installation instructions:
https://learn.adafruit.com/adafruit-pioled-128x32-mini-oled-for-raspberry-pi/usage
Note section on “Speeding up the display”


eBay source for 128x32 OLED (USA seller)
http://www.ebay.com/itm/0-91-128x32-I2C-IIC-OLED-LCD-White-Display-DIY-Module-3-3V-5V-For-PIC-Arduino/172683183086

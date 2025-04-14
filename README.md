# NTP Dual Clock with Solar Conditions
This is a modified version of ESP-8266 sketch for Bruce Hall's NTP Clock architecture. 
The sketch in this project is designed for a kit project of the CalQRP Club (calqrp.groups.io). 
It adds the current solar flux, A-index and K-index as provided by hamqsl.com's XML service.
The following changes were made to Burce Hall's original implementation:

- For my own preference I changed the A/P indicator to show a full AM/PM for 12 hour modes.

- I moved the #defines for your wifi network to a separate include file. This enable easier
updating of the github repo as while I'm testing code with my credentials I can just commit
changes to the main file without constantly editing credentials. Make sure this include
file is in the same directory as the main .ino file.

- Of course I also added the solar data.

A few details about how the solar data is obtained: When you first start the clock the code 
takes the current second and uses a simple
heuristic to construct a mostly random second. The the second modulo 5 is used to generate
a corresponding minute. This minute is further incremented by 1 to start 1 minute after the hour.
Why all this fuss? The SFI is presuably changed on the hour so we want to start a bit later than that.
Further if more than one of us uses this code we don't want to all try to grab the data at the same
time. I don't know how well this will work in practice so give me feedback if you run into issues.

So the bottom line is the code currently grabs the data once an hour on some point between minute 1 and minute 6.

If you check the clock data with QRZ which displays the same hamqsl.com data, be aware that they update less frequently
than hamqsl.com itself. So you may see temporary discrepencies with QRZ.com.

Good luck and 73
Robert, AI6P

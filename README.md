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

- There is a new library depedencies. You will need to add ESP8266HTTPClient:<BR>
https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266HTTPClient<BR>
It's also available to add from the IDE Library UI.

- Of course I also added the solar data. This data comes from [hamqsl.com](https://www.hamqsl.com/solarxml.php)
and is the same source used by many websites including QRZ.com.

NOTE: I can only test the 8266 version. But I added the corresponding HTTPCLient to the ESP32 option.
It should work ok, but just be aware it hasn't been tested or even compiled for ESP32.

A few details about how the solar data is obtained: When you first start the clock the code 
takes the current second and uses a simple
heuristic to construct a mostly random second based on when you started the app. The the second modulo 5 is used to generate
a corresponding minute. This minute is further incremented by 32 to update sometime after 2 minutes after the half hour (to give
hamsql.com time to propogate any changes).
Why all this fuss? After monitoring for some time, I determined
that a few minutes after the 1/2 hour each hour was the best time to update hopefully catch the latest updates. 
By spreading out individual instances to semi-random time slots slots I hope to avoid any issues or
request delays that might happen if a sufficient number of folks use this code and run it 24/7 and they all ask for
the data at EXACTLY the same time. The whole point of NTP Clock is to get a syncronized time after all.


So the bottom line is the code currently grabs the data once an hour on some point between minute 1 and minute 6.

If you check the clock data with QRZ which displays the same hamqsl.com data, be aware that they update less frequently
than hamqsl.com itself. So you may see temporary discrepencies with QRZ.com.

Good luck and 73
Robert, AI6P

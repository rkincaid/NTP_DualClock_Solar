# NTP Dual Clock with Solar Conditions
![image](https://github.com/user-attachments/assets/5e51f175-e064-4b67-95a2-e13680797d09)

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

- **This version HAS been modified to work properly with the ESP32** and
should work ok. This is true even for John Price's PCB (different from the CalQRP board).

- This version also includes the text for **Geomagnetic Field** which is shown on the upper clock (see photo above)

A few details about how the solar data is obtained: When you first start the clock the code 
takes the current second and uses a simple
heuristic to construct a mostly random second based on when you started the app. The the second modulo 5 is used to generate
a corresponding minute. This minute is further incremented by 32 to update sometime after 2 minutes after the half hour (to give
hamsql.com time to propogate any changes).
Why all this fuss? After monitoring for some time, I determined
that a few minutes after the 1/2 hour each hour was the best time to update in order to hopefully catch the latest data. 
By spreading out individual instances to semi-random time slots slots I hope to avoid any issues or
request delays that might happen if a sufficient number of folks use this code and run it 24/7 and they all ask for
the data at EXACTLY the same time. The whole point of NTP Clock is to get a syncronized time after all.

If you check the clock data with QRZ which displays the same hamqsl.com data, be aware that they update less frequently
than hamqsl.com itself. So you may see temporary discrepencies with QRZ.com. The same is true between hamqsl.com, QRZ 
and the actual NOAA sources. Eventually, the clock will refresh with the appropriate solar data. I did try to optimize this
as best I could.

Good luck and 73
Robert, AI6P

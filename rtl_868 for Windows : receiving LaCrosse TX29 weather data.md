# rtl_868 for Windows : receiving LaCrosse TX29 weather data

This article is a part of a whole global project : build my own weather station, based on Lacrosse weather sensor. 
For this project, I have to build a software decoder to retreive weather data from the radio sensor. 
I tested several software, including a program called RTL_868. This program was only for Linux or MacOSX, and I provide in this article a Windows version.

The original version of RTL_868 was made by Git Hub user [“sum-sum”, and source code is available on Git Hub](https://github.com/sum-sum/rtl_868).
The source code can not be compiled out-of-the-box for Windows, so thanks to Git and the GPL Licence, [I forked the project to create a Windows version of rtl_868](https://github.com/psa-jforestier/rtl_868).
Now, I can build a Windows version from a Linux box, by doing cross-compilation.

I used to have Win32 binary executables build for you, but I delete them and I'm not able to rebuild them... Sorry !

To use it, you must have the RTL SDR tool box, for Windows with the FM demodulator (works also with AM).
Download and install the Windows version of rtl_868 in the same directory as the “rtl_fm.exe” binary you installed.

Then you can use the following command line to decode Lacrosse TX29+IT radio sensor (transmission on 868,2Mhz) :
```
rtl_fm.exe -f 868.26e6 -M fm -s 500k -r 75k -g 42 -A fast - | rtl_868.exe
-f 868.26e6 : frequency (could be adjusted depending of your sensor)
-M fm : modulator
-s 500k : sampling rate
-r 75k : resampling rate (it is the real sample rate of the outputed data)
-g 42 : gain (should be adjusted from 0 to 50)
-A fast : math method used by the RTL dongle
```

Then, after a few seconds, you will see something like :

```
Found 1 device(s):
0: Realtek, RTL2838UHIDIR, SN: 00000001
Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner
Tuner gain set to 42.10 dB.
Tuned to 868635000 Hz.
Oversampling input by: 3x.
Oversampling output by: 1x.
Buffer size: 5.46ms
Exact sample rate is: 1500000.014901 Hz
Sampling at 1500000 S/s.
Output at 500000 Hz.
2016-12-08 21:04:20, 1481227460, 22, 8.90, nan, 0.
2016-12-08 21:04:21, 1481227461, 33, 21.00, nan, 0.
2016-12-08 21:04:24, 1481227464, 60, 3.90, nan, 0.
```
On each line, you see : the date, the timestamp, the sensor IR, the temperature (in °C), the hygromethry, and a flag (1 if low battery).

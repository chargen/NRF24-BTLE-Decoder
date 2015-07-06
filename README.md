NRF24-BTLE-Decoder
==================

Sniff and decode NRF24L01+ and Bluetooth Low Energy using RTL-SDR.  
These protocols use the ISM 2.4Ghz frequency range, which is beyond the capabilities of the cheap rtl-sdr, a down convertor is necessary. See http://blog.cyberexplorer.me/2014/01/sniffing-and-decoding-nrf24l01-and.html for more details.

The main repository is at https://github.com/omriiluz/NRF24-BTLE-Decoder

Compile
-------
`make`  
or directly  
`gcc -std=gnu99 -Wall -O3 -o nrf24-btle-decoder nrf24-btle-decoder.c`

Usage
-----
`nrf24-btle-decoder [-t nrf|btle|raw] [-d 1|2|8] [-l len] [-c 0|1|2]`
-t packet_type (nrf or btle or raw), defaults to nrf. Using packet type btle implies -d 2  
-d downsample_rate (1 for 2mbps, 2 for 1mbps, 8 for 256kbps), default to 2  
-l len (1-32). Sets a fixed packet length
-c byte length of the CRC, default to 2

Important - this program input is a 2M samples per second bitstream generated by rtl_fm or equivalent e.g. rtl_fm.exe -f 428m -s 2000k | nrf24-btle-decoder.exe -t nrf -s 2

New: raw mode is for use without the Enhanced ShockBurst packet control field. 
Length must be specified for this mode. 
It verifies packets based on CRC, so if in the future, the code is given the option for no CRC, this becomes problematic. An option for a fix is match against an address or partial address. This is probably a reasonable feature to add anyway. 


Dependencies
------------
* working rtl-sdr library. See http://sdr.osmocom.org/trac/wiki/rtl-sdr
* working hardware - rtl-sdr, downconverter, antenna

Limitations
-----------
* The BTLE protocol decoder currently supports only advertisement packets on channel 38 and not data packets / frequency hopping. I am still evaluating whether the rtl-sdr hardware is fast enough to track the frequency hopping.

Troubleshooting
-----------------
* Biggest problem is noise, avoid rf auto gain and set as low as possible. I usually get best results with 0-10 db gain.  
* Second biggest problem is frequency drift. Use kalibrate for a good base line then fine tune the frequency in 50Khz steps until perfect  

License
-------
All of the code contained here is licensed by the MIT license.

Credit
------
Dmitry Grinberg, CRC and Whiten code for BTLE - http://goo.gl/G9m8Ud  
Open Source Mobile Communication, RTL-SDR information - http://sdr.osmocom.org/trac/wiki/rtl-sdr  
Steve Markgraf, RTL-SDR Library - https://github.com/steve-m/librtlsdr  

-----------------

Copyright (c) 2014 Omri Iluz (omri@il.uz / http://cyberexplorer.me / https://github.com/omriiluz)

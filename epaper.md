# Hardware
https://www.aliexpress.us/item/3256805623572150.html?spm=a2g0o.order_list.order_list_main.5.21ef1802QMKkkb&gatewayAdapt=glo2usa
https://www.aliexpress.us/item/3256804935498922.html?spm=a2g0o.order_list.order_list_main.11.21ef1802QMKkkb&gatewayAdapt=glo2usa

# First Use (software)
https://www.waveshare.com/wiki/E-Paper_ESP32_Driver_Board#Demo_Usage
doc: https://www.waveshare.com/wiki/7.5inch_HD_e-Paper_HAT_(B)

* The hardware came with the A/B switch set to B.  Doc says most things (and the 7.5") work on A, so I flipped that
* Connections are all straight-through, everything face up.  
* The example that worked is edp7in5b_V2.ino.  If flashes a bit, and after about 10 seconds, you get the "Waveshare" logo in black, then with red.
* 

## Current status
~~I'm working in PullAndPaint-01.  Things clear, I'm working on getting rid of the crazy boot sequence. The json comes from the npm start of policy-resources (art01.yaml).  But it's not actually writing to the screen.~~

I'm working in PullAndPaint-bitmap at https://github.com/sweeneyb/png-to-epaper/.  I have it pulling 2x pngs and decoding them.  I think there's an allocation in the display that's blowing out memory.  I can paint one thing, but not 2x.  

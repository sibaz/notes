# Lora Notes

So, I have bought:-

1) https://thepihut.com/products/sx1262-lora-hat-for-raspberry-pi-868mhz-for-europe-asia-africa
1) https://thepihut.com/products/sx1262-868mhz-lora-node-module-for-raspberry-pi-pico 

Which are respectively the rpi5/rpi5-zero hat and pico2 hat, for the SX1262 868M is a LoRa node expansion module

Digging the official docs has got me to a few things, none of it, looks terribly helpful:-
1) https://www.waveshare.com/wiki/Pico-LoRa-SX1262-868M
1) https://github.com/siuwahzhong/lorawan-library-for-pico
1) https://www.waveshare.com/wiki/SX1262_868M_LoRa_HAT#Support
1) https://files.waveshare.com/upload/c/c4/SX1268_V1.0.pdf
1) https://files.waveshare.com/upload/6/62/DS_SX1261-2_V1.1.pdf
1) https://files.waveshare.com/upload/a/af/SX1268_LoRa_HAT_SchDoc.pdf
1) https://github.com/rzeczpospolitanski/MeshCore-RaspberryPi-Zero2-SX126X
1) https://meshcore.co.uk/flasher.html
1) https://github.com/sbcshop/USB_Type_C_to_LoRa_Dongle_Software
1) https://shop.sb-components.co.uk/products/usb-type-c-to-lora-dongle?variant=41026476769363

# TL;DR

In essence it seems the lora hat communicates via rpi, using a serial connection, where the raspi-config is needed to disable the serial console, and instead enable the serial device so coms can happen.  

I've seen examples to set this up in python, but all seem to assume a tx and rx pair, and I've only got x1 to work, so I'm not getting much joy.  Hence I'm considering sourcing a bog standard usb lora device, ideally also using sx1268, just to set this up on a regualt ubuntu laptop to see if I can get it to work at all first

it remains to be seen, what sort of actual connectivity is available.  My gut is, this isnt going to manifest as a hardware/kernel module, but instead require user mode coms with the serial device to actually make it work.  That fits with the fact that these pi hat devices seem to have independant on/off switches and batteries



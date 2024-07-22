# micropython_ethernet

## BackStory
- Can I get 2 microcontrollers to use micropython to communicate over Ethernet?
- Turns out the answer is yes
- Your mileage may vary depending on what you want to do

##  Prerequisites

| Hardware | Link | 
|---|---|
|2 x Raspberry pi Picos|Amazon|
|USR-ES1 W5500 Ethernet Network Module Hardware SPI to LAN/ Ethernet TCP ... W5100"|[AKA "Wiznet 5500](https://www.aliexpress.us/item/3256804674673261.html):

I had success with these:
 ![image](https://github.com/user-attachments/assets/d7ae2a9f-5037-468d-88e9-c5acaacc438a)

## Installation 
- Download the latest Wiznet Firmware [here](https://micropython.org/download/W5500_EVB_PICO/)
- Install it by copying the u2f per usuall after holding the BOOTSEL button and powering on the pico via USB.
- It should be rebooted and then show:
  
![image](https://github.com/user-attachments/assets/8711fd1a-3bd2-4306-9237-ed97f1da5453)


## Pin Configuration

The pins are very clearly documented under the [WizNet Docs](https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/Ethernet%20Example%20Getting%20Started%20%5BMicropython%5D.md):

![image](https://github.com/user-attachments/assets/c3975716-31a0-4755-ac59-04e6be781a35)



## Full Micropython Example


```
import time
import network
from machine import Pin,SPI

spi=SPI(0,2_000_000, mosi=Pin(19),miso=Pin(16),sck=Pin(18))
nic = network.WIZNET5K(spi,Pin(17),Pin(20)) #spi,cs,reset pin
nic.active(True)
nic.ifconfig(('192.168.0.249', '255.255.255.0', '192.168.0.197', '8.8.8.8'))
time.sleep(2)
print(nic.isconnected())
print(nic.regs())
print(nic.ifconfig())
print(nic.active())

```

NOTES:

```
If you use the Dynamic IP(DHCP), you must use the "nic.ifconfig('dhcp')" syntax:
    nic.ifconfig('dhcp')
    
If you use the Static IP, you must use the  "nic.ifconfig("IP","subnet","Gateway","DNS")".
    nic.ifconfig(('192.168.100.13','255.255.255.0','192.168.100.1','8.8.8.8'))
   
```


## Takeaways/ Learnings
- I liked the small form factor better as they fit really nicely on the bread board for testing.
- I tried using a 6 inch Ethernet cable, but that failed
- I was able to use a 12 inch Ethernet cable
- I don't actually suggest to connect 2 MCUs on a breadboard via ethernet, it's just too big and unwieldy, but it was fun.
  

## References

| Topic | Link | 
|---|---|
|Is any type of Ethernet supported on ESP32 platforms now?|[micropython forum](https://github.com/orgs/micropython/discussions/11474)|
| Using Micropython to connect Wiznet W5500 Pico Pis over Ethernet | [Stephanj’s Writings](https://sjhennion.github.io/jekyll/update/2023/09/22/w5500-intro.html)|

## License
This project is licensed under the [MIT License](LICENSE).
Feel free to modify the content as needed, such as adding installation instructions, code examples, or any other relevant information for your project.



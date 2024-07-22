# micropython_ethernet

## BackStory
- Can I get 2 microcontrollers to use micropython to communicate over Ethernet?
- Turns out the answer is yes
- Your mileage may vary depending on what you want to do, I tried 2 different options

##  Option 1 - Prerequisites

| Hardware | Link | 
|---|---|
|2 x Raspberry pi Picos|Amazon/Anywhere|
|USR-ES1 W5500 Ethernet Network Module Hardware <br> SPI to LAN/ Ethernet TCP ... W5100"|[AKA "Wiznet 5500](https://www.aliexpress.us/item/3256804674673261.html)|
|Crossover Cable (If not using Crossover Adapter)|Almost any will do|
|Crossover Adapter (optional if using Cable)| [Amazon](https://www.amazon.com/gp/product/B01I0E5EXU)

I did not see anyone online using the "USR-ES1 W5500 Ethernet Modules" nor did anyone advertise them per se. <P>
I stumbled upon them on Aliexpress and noticed the markings were the same as the Wiznet Board.<P>
In the end, I had success with them:<P>

<img src="https://github.com/user-attachments/assets/d7ae2a9f-5037-468d-88e9-c5acaacc438a" width="200" height="150">


I had success with these Cable Matters cross over adapters:<P>

<img src="https://github.com/user-attachments/assets/2c0a3fe7-2239-4ae2-892a-c16ace24cfef" width="200" height="150">

## Power
- I connected the Ethernet modules's VCC and GND to an external breadboard power supply (max 700 MA), not the pico.
  
## Installation 
- Download the latest Wiznet Firmware [here](https://micropython.org/download/W5500_EVB_PICO/).
- Install it by copying the u2f per usuall after holding the BOOTSEL button and powering on the pico via USB.
- It should be rebooted and then will show the WIZNET version:
 ![image](https://github.com/user-attachments/assets/e4dd657c-fd20-4f5a-af1e-9a52be06f61b)

## Pin Configuration

The pins are very clearly documented under the [WizNet Docs](https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/Ethernet%20Example%20Getting%20Started%20%5BMicropython%5D.md):

Souce: https://github.com/user-attachments/assets/c3975716-31a0-4755-ac59-04e6be781a35
![image](https://github.com/user-attachments/assets/c3975716-31a0-4755-ac59-04e6be781a35)


## Connecting via DHCP or Static IP

NOTES:

```
If you use the Dynamic IP(DHCP), you must use the "nic.ifconfig('dhcp')" syntax:
    nic.ifconfig('dhcp')
    
If you use the Static IP, you must use the  "nic.ifconfig("IP","subnet","Gateway","DNS")".
    nic.ifconfig(('192.168.100.13','255.255.255.0','192.168.100.1','8.8.8.8'))
   
```

You should set both to static if they are connecting only to each other. 

*IMPORTANT:* You must use a cross over cable or cross over adapter to connect both together.

You can try DHCP if you connect one of them up to a LAN w/a DHCP server.

*IMPORTANT:* You'll likeley need at least a 12 inch Ethernet cable.


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

## Code Examples

- Once both are having static IPs as above you can connect via HTTP or other means. The [Wiznet docs](https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/Ethernet%20Example%20Getting%20Started%20%5BMicropython%5D.md#ethernet-example-structure) are quite good.
- If connected to a LAN, you could ping each of the picos.
- I am unaware of a ping utility specifically for the Wiznet Firmware, however. (see References below for Discussions regarding Ethernet communication and the Wiznet)
 
## Takeaways/ Learnings
- I liked the small form factor better as they fit really nicely on the bread board for testing.
- I tried using a 6 inch Ethernet cable, but that failed.
- I was able to use a 12 inch Ethernet cable.
- I don't actually suggest to connect 2 MCUs on a breadboard via ethernet, it's just too big and unwieldy, but it was fun:

The cable takes a bit of space:

<img src="https://github.com/user-attachments/assets/a99fcf79-7716-44c1-b38a-67477481a636" width="350" height="500">

- Don't be afraid to the run jumper cables a bit. This setup is for another project so I had to run the cables long ways in 2 'jumps':

<img src="https://github.com/user-attachments/assets/35ed0b69-354a-4edc-a289-e3cc02ad2ae6" width="350" height="500">


##  Option 2 - Prerequisites
| Hardware | Link | 
|---|---|
|Raspberry pi Picos|Amazon/Anywhere|
|TZT ENC28J60 SPI interface network module Ethernet <br> module (mini version) for arduino |[AliExpress](https://www.aliexpress.us/item/3256805818734279.html) |

## Installation 
- Install 'normal' micropython per usual.

## Code
- Upload `enc28j60.py` from https://github.com/przemobe/micropy-ENC28J60/tree/main
 
## Pin Configuration
- Follow https://github.com/przemobe/micropy-ENC28J60/tree/main

## Code Examples
- Follow https://github.com/przemobe/micropy-ENC28J60/tree/main

## Takeaways/ Learnings
- This repo 'just worked' the first time, was LOW hassle to get working and great documenation.
- That said I could send a 'raw' ethernet packet following the author's instructions:

![image](https://github.com/user-attachments/assets/93d43ed5-8b57-4b97-8ddd-6ff8a1a0f467)

![image](https://github.com/user-attachments/assets/9eaffa9a-2325-4183-a9ab-4d30737b3edb)

- I did not to get ping working, mainly once the Wiznet's arrived I noticed they worked out better on the broadboard and I moved on.
- It seems like it's possible though.
 
## References

| Topic | Link | 
|---|---|
| Is any type of Ethernet supported on ESP32 platforms now?|[micropython forum](https://github.com/orgs/micropython/discussions/11474)|
| Using Micropython to connect Wiznet W5500 Pico Pis over Ethernet | [Stephanj’s Writings](https://sjhennion.github.io/jekyll/update/2023/09/22/w5500-intro.html)|
| Wiznet Ethernet Function Discussion| [Github](https://github.com/micropython/micropython/issues/15170)|

## License
This project is licensed under the [MIT License](LICENSE).
Feel free to modify the content as needed, such as adding installation instructions, code examples, or any other relevant information for your project.



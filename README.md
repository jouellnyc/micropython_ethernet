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


## Pin Configuration

The pins are very clearly documented under the [WizNet Docs](https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/Ethernet%20Example%20Getting%20Started%20%5BMicropython%5D.md):

![image](https://github.com/user-attachments/assets/c3975716-31a0-4755-ac59-04e6be781a35)



## Full Micropython Example

## Takeaways/ Learnings
- I liked the small form factor better as they fit really nicely on the bread board for testing.
- I tried using a 6 inch Ethernet cable, but that failed
- I was able to use a 12 inch Ethernet cable

## References
| Topic | Link | 
|---|---|
|Is any type of Ethernet supported on ESP32 platforms now?|[micropython forum](https://github.com/orgs/micropython/discussions/11474)|
| Using Micropython to connect Wiznet W5500 Pico Pis over Ethernet | [Stephanj’s Writings](https://sjhennion.github.io/jekyll/update/2023/09/22/w5500-intro.html)|

## License
This project is licensed under the [MIT License](LICENSE).
Feel free to modify the content as needed, such as adding installation instructions, code examples, or any other relevant information for your project.



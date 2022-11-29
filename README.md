# HomeAssistant GarageSensor remote control

This Lab’s main focus is to create some sort of remote control of the garage system consisting of all of the devices within the garage that have been created so far (see Labs 1-5).  This lab will be using Apple’s Siri and Apple’s Homekit to connect the Home assistant set up to a mobile device that can be controlled through voice activation.

## Requirements
- Use three Arduino Wemos D1 Mini’s
- Use a breadboard, LEDs (red, green, yellow, blue), and resistors to create one of the circuits
- Use a breadboard and a distance sensor to create the second circuit
- Use a breadboard and a magnetic sensor to create the third circuit
- Run home assistant on any available platform
- Connect the arduino’s to home assistant
- Add some third party remote control device to make things easier than pressing a button
- When the distance sensor senses something very close, the red light flashes
- When the distance sensor senses something close, the red light turns on
- When the distance sensor senses something at a medium distance, the yellow light turns on
- When the distance sensor senses something at a larger distance, the green light turns on
- When the distance sensor senses anything within that larger distance, the blue LED illuminates
- When the distance to sensor exceeds a large reasonable distance, any illuminated LED or current blink mode is stopped and the machine is in the off state
- Reasonable time interval for each flash in the blink mode is used

## System View

The user facing interface for this system is a physical stoplight (Figure 1) that is visible to the user, a distance sensor (Figure 2), and a magnet sensor (Figure 3), and the Home Assistant UI (Figure 4), and Apple’s IOS (Figure 5) to interact with. The user will be able to move an object at varying distances from the sensor and be able to witness different colours of lights that will indicate how far away that object is. The user will also be able to connect and disconnect the magnet sensor to turn the light system on and off.  This is representative of a garage parking distance sensor.  As the user moves into the garage, the light will change based on distance sensor readings and the status of the garage door (magnet sensor).  A distant user will also be able to witness the status of all of the devices through the Home Assistant UI, allowing them to keep an eye on their garage.

<img width="75" alt="Screenshot 2022-11-15 at 3 31 17 PM" src="https://user-images.githubusercontent.com/59840208/202050496-bf03db21-bc30-4984-8541-a3ee4bc9b906.png">
Figure 1

<img width="464" alt="Screenshot 2022-11-15 at 5 03 35 PM" src="https://user-images.githubusercontent.com/59840208/202050611-f751200a-a158-45df-b539-b98d4745a70d.png">
Figure 2

<img width="381" alt="Screenshot 2022-11-15 at 5 03 47 PM" src="https://user-images.githubusercontent.com/59840208/202050631-b0e14da1-193e-45c4-a10e-3f4ed1b64146.png">
Figure 3

<img width="1781" alt="Screenshot 2022-11-15 at 2 56 04 PM" src="https://user-images.githubusercontent.com/59840208/202050697-0598a527-2028-468d-a662-c17150dacffb.png">
Figure 4

The User Interface on Home Assistant allows the user to see the exact distance an object is away from the sensor, whether or not the garage door is opened or closed, the status of the light used for parking and each individual LED on it, and to see whether the car is in the garage.  The system is set to change based on events from these entities, but the distant user has access to change certain components from this interface.

![IMG_20D6A22A6467-1](https://user-images.githubusercontent.com/59840208/204441612-037d34c7-ac62-4fb8-88f8-9b1634c6ac91.jpeg)
Figure 5

Through Apple’s Siri and Homekit, these devices can be controlled even easier through touch and voice commands.  When connected to Home Assistant, the garage section on the interface will visually show the status of any of the components, automations, or variables that may want to be monitored or controlled through the app or through Siri.  On top of this, each component has it’s own name that can be controlled through Siri.  For example, saying “Hey Siri, turn on the garage door” will turn on that first boolean in the garage section of the interface, indicating that the garage door is now open and the rest of the system will activate.   

## Component View

There are four main components for this lab.  There is the raspberry pi, the stop light, the distance sensor, and the magnet sensor.

For the raspberry pi (figure 6), a few changes had to be made to the pi.  The Micro SD card was removed and the software belenaEtcher was used to flash the Home Assistant software onto the pi.  After waiting for the system to boot up, the system is ready to go on http://homeassistant.local:8123 on your computer connected to the same network.  This webpage is what is used to hook up devices, configure the functionality of all the devices, and add rules dependent on the state each device is in.  The main dashboard is customizable to be able to suit the user’s needs.

<img width="439" alt="Screenshot 2022-11-15 at 5 06 11 PM" src="https://user-images.githubusercontent.com/59840208/202050902-621bbd86-fd65-45c5-a845-a786a35df757.png">
Figure 6

The circuit for the stoplight (figure 7) has a few parts.  The main driver of the whole system is the Wemos arduino D1 mini.  This is programmed through a yaml file that is installed on the arduino.  This yaml file is generated through the Home Assistant user interface and edited to read the sensor before sending to the arduino.  The yaml can be installed through various methods, but the method used for this lab was by connecting the arduino to the raspberry pi, and selecting the method to install using a wired connection to a platform hosting the Home Assistant software.  There are also some LED lights (red, yellow, and green) that indicate the different distances from the sensor.  The red LED is connected to pin D6, the yellow LED is connected to pin D5, and the green is connected to pin D0.  There is a blue LED as well that indicates whether there is a vehicle in the garage. There are also resistors that are provided so that the LEDs do not blow from too much voltage.  These are connected to the ground wire, connected to the ground pin. Lastly, we have the power source which is connected to the arduino through a micro usb cable.

<img width="318" alt="Screenshot 2022-11-15 at 5 06 51 PM" src="https://user-images.githubusercontent.com/59840208/202050990-d8fb7bcf-d429-40cc-ae2a-d2c0b61f1c88.png">
Figure 7

<img width="484" alt="Screenshot 2022-11-15 at 4 55 39 PM" src="https://user-images.githubusercontent.com/59840208/202051009-38df863b-c2bb-473b-ac1e-608a4acb96da.png">
Figure 8 (battery represents arduino)

The circuit for the sensor (figure 9) also has a few parts.  Like the stoplight, a Wemos arduino D1 mini is also programmed using a yaml file (see appendix).  The other part of this circuit is the sensor (HC-SR04) itself.  The sensor will read in data from sending a signal and reading the echo of the sensor.  The last part is a power source which connects to the arduino through a micro usb cable.

<img width="268" alt="Screenshot 2022-11-15 at 5 07 41 PM" src="https://user-images.githubusercontent.com/59840208/202051095-3f7d2d16-b0ea-4abd-87ac-a75964133387.png">
Figure 9

<img width="246" alt="Screenshot 2022-11-15 at 5 08 00 PM" src="https://user-images.githubusercontent.com/59840208/202051145-d3dbccb3-1e19-4f8d-b50e-576140d82445.png">
Figure 10 (Wemos D1 Mini replaced with Nano)

The last component, the magnet sensor (Figure 11) is fairly straightforward. Also programmed with a yaml file, the Magnet sensor has two parts to connect, and one side has two wires.  These wires were stripped and taped to some extenders for a better connection.  One wire was then plugged into the ground pin on the arduino and the other on the D4 pin.  Once configured with programming, the arduino will be able to detect whether the sensor is closed or open with a high (open) or low (closed) reading.

<img width="367" alt="Screenshot 2022-11-15 at 5 10 39 PM" src="https://user-images.githubusercontent.com/59840208/202051483-56ae3930-2ee8-4ebe-9134-c80894bca25b.png">
Figure 11

<img width="373" alt="Screenshot 2022-11-15 at 5 10 42 PM" src="https://user-images.githubusercontent.com/59840208/202051505-0a8e849e-2013-4ea3-805b-137eeb5c37f7.png">
Figure 12 (Wemos D1 Mini replaced with Nano)

To connect the devices to Home Assistant, the Home Assistant UI needed to be used.  The Arduino IDE did not need to be used at all with this lab as all functionality is done through Home Assistant.  

There are multiple possible approaches for connecting all of the devices, but for this lab, esphome was used.  Details on how Esphome was set up and connected, see lab 5.  

For this lab, more had to be added to Home Assistant.  In settings -> devices & services -> helpers, a new toggle helper (input boolean) was created to add a nice interface to turn the garage on(open)  and off (closed).  Then a few automations were tweaked to toggle this based on the magnet sensor’s state so the two could be linked.  When the magnet opens, the toggle turns on and when the magnet closes, the toggle turns off.

To integrate Home Assistant with Apple’s Homekit so the device can be controlled through Siri, start by adding a new integration.  In Settings -> devices & services -> integrations, select “Add Integration” in the bottom corner (figure 13).  Type in “Apple” and select the “Apple” option (figure 14), and then search and select the “Homekit” option (figure 15).  Configure preferences (figure 16)  and press submit.

<img width="1519" alt="Screenshot 2022-11-25 at 3 42 48 PM" src="https://user-images.githubusercontent.com/59840208/204441955-453ca5b8-303d-4670-9d90-1c582ba54956.png">
Figure 13

<img width="500" alt="Screenshot 2022-11-25 at 3 42 58 PM" src="https://user-images.githubusercontent.com/59840208/204442005-fec97b0b-d3da-45ee-9090-592bcd355b8a.png">
Figure 14

<img width="493" alt="Screenshot 2022-11-25 at 3 43 09 PM" src="https://user-images.githubusercontent.com/59840208/204442059-aff21878-8626-4e1a-9699-06061ad0d3d9.png">
Figure 15

<img width="585" alt="Screenshot 2022-11-25 at 3 43 19 PM" src="https://user-images.githubusercontent.com/59840208/204442077-79a5ba45-0145-485c-8ada-fbb0182de92a.png">
Figure 16

Next, head to the notifications tab in the side bar and there should be a QR code (figure 17).  Download Homekit onto an IOS device and press the + icon in the top right corner (figure 5).  Select “Add Accessory” and scan the QR code on your web interface until it recognizes the device.

<img width="466" alt="Screenshot 2022-11-25 at 4 15 56 PM" src="https://user-images.githubusercontent.com/59840208/204442138-d4a5c74f-69c5-4a09-8aa6-d673de43eda1.png">
Figure 17

The interface on the phone will then walk you through setting up the different devices.  It also includes automations.  Be sure here to include your input boolean and give it a recognizable name.  “Garage door” was used for this lab since this is what is representing the door.  Once set up, add whatever devices or automations needed in the home panel as easily accessible, and then asking Siri to control things using the correct names (“Hey Siri, turn off garage door”) will control the garage light device according to the automations created.

## Thought Questions

1. What services did you consider integrating into your project and why?
  - I mostly just considered Siri since I use Apple products a lot and don’t own any other device with voice commands.  I thought voice is most likely one of the easiest thing to use other than pressing a button and is also accessibility friendly.
2. What services would you like to integrate in the future?
  - Motion detection would be cool to look at, however, for purposes of a garage door that would be highly insecure.
3. Consider internal and external security threats. Identify 3 likely attack vectors for someone to compromise this system? How did you mitigate these?
  - This currently doesn’t track who’s voice is who’s.  This means anyone with access to the phone can change the garage lights.  This could be mitigated by changing the settings of Siri to only allow activation when locked, although this may affect the convenience
  - Outdated HA patches:  If you are not frequently updating your home assistant software, security risks from previous patches become a risk.  Since the main interface is Siri, there is not a lot to suggest frequent updates and send quick notification reminders.  Outdated versions become a big risk in this way but this can be mitigated by keeping on top of updates yourself.
  - Outdated integrations also pose a large security risk.  For example, if font awesome is used to add icons to the HA dashboard, it may look nice, but FontAwesome is a very outdated library that hasn’t been maintained.  This causes a lot of risk regarding security.  Mitigate this by keeping an eye on the security of each integration that is being added to HA. 
4. What was the biggest challenge you overcame in this lab?
  - Spent a lot of time trying to get the lights to turn on immediately after opening garage without changing colour zones first.  This was not successful.
5. Please estimate the total time you spent on this lab and report.
  - 4-5 hours

## Verification

This lab is the work of Anaïs Dawes.  Any resources used are listed below in “References”.

## References

Getting Home Assistant installed and working: https://www.home-assistant.io/getting-started/

Installing Home Assistant on Pi: https://www.balena.io/etcher/

Getting ESPHome running: https://iotdesignpro.com/projects/getting-started-with-esphome-how-to-install-and-integrate-with-home-assistant

Hooking up Home Assistant to IOS: https://www.johnkeen.tech/using-siri-apple-homekit-with-home-assistant/

Creating Input Booleans: https://www.home-assistant.io/integrations/input_boolean/

Communication troubleshooting: Berin Hamilton, IT&C student

## Appendix

GPIO pins: 
https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/

<img width="773" alt="Screenshot 2022-11-15 at 5 16 14 PM" src="https://user-images.githubusercontent.com/59840208/202052170-98d04693-c519-4552-af85-5c6c5993815a.png">

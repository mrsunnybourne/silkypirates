# eSP Deauthenticator
Deauthentication attack and exploits using an ESP8266 NodeMCU Chip	

<img width="100%" alt="esp8266 deauther with smartphone" src="https://raw.githubusercontent.com/spacehuhn/esp8266_deauther/master/screenshots/smartphone_and_deauther.jpg">

<p align="center">
<a href="https://www.linkedin.com/in/sandeep-kadam/">Sandeep Kadam</a>
| <a href="https://www.facebook.com/yogesh.sravan?ref=br_rs">Yogesh Sathuluri</a>
| <a href="https://www.facebook.com/saisuryanarayanaraju.saripella">Suryanarayana Saripella</a><br/>
<br />
</p>

## Contents
- [Introduction](#introduction)
  - [What it is and how it works](#what-it-is-and-how-it-works)
  - [How to protect yourself against it](#how-to-protect-yourself-against-it)
- [Disclaimer](#disclaimer)
- [Installation](#installation) 
  - [Uploading the bin files](#uploading-the-bin-files)  
  - [Compiling the source with Arduino IDE](#compiling-the-source-with-arduino)
- [How to use it](#how-to-use-it)
- [License](#license)


## Introduction

### What it is and how it works

This application allows you to perform a [deauth attack](https://en.wikipedia.org/wiki/Wi-Fi_deauthentication_attack) with an [ESP8266 WiFi Module](https://en.wikipedia.org/wiki/ESP8266) against selected networks.  
The ESP8266 is a cheap and easy to use Wi-Fi SoC (System-on-a-Chip), programmable with the [Arduino IDE](https://www.arduino.cc/en/Main/Software).  
With our code flashed onto it, you can select a target network and start different deauthentication attacks.

The deauth attack will, if the connection is vulnerable, disconnect the devices from the network. Because the attack is running constantly, the devices will be disconnected again and again. Depending on the network, that can either block a connection or slow it down.  

Other attacks also have been implemented, such as beacon flooding and probe request flooding.

The deauth attack works by exploiting an old and known vulnerability in the 802.11 Wi-Fi protocol.  
Because these [deauthentication frames](https://mrncciew.com/2014/10/11/802-11-mgmt-deauth-disassociation-frames/), usually used to close a Wi-Fi connection safely, and are unencrypted, it's very easy to spoof them. You only need the mac address of the access point, which you can sniff easily.  
If you don't want to attack all connected devices, you can also scan for connections and attack them specifictly.  

### How to protect yourself against it

With [802.11w-2009](https://en.wikipedia.org/wiki/IEEE_802.11w-2009) the Wi-Fi protocol became encrypted management (and deauthentication) frames. This makes spoofing these packets way harder and the attack, in this form, ineffective.
So make sure your router is up to date and has management frame protection enabled. Your client device (e.g your Smartphone, Laptop etc.) needs to support that too. Both ends of the connection need to use it!

The problem with that is, most routers use unencrypted management frames by default, and don't provide any option to change that and don't provide any information about this issue.  
We tested few networks(due to time) and couldn't find any one that wasn't vulnerable!  

## Disclaimer

**This project is a proof of concept for testing and educational purpose only.**  
Neither the ESP8266, nor its SDK was meant or build for illegal purposes.  
Bugs can occur for some people!
  
## Installation

**You have 2 ways here:**

**1** Uploading the .bin files is easier, but not as good for debugging.  
**2** You need to have an Arudino IDE on your workspace/system and can debug our code from it.

**YOU ONLY NEED TO DO ONE OF THE INSTALLATION METHODS!**  

### Uploading the bin files  

**1** Download the current release from [releases](https://github.com/mrsunnybourne/silkypirates/tree/master/bin%20files)  

**Recommended:** Use the 1mb version, unless you're sure that your ESP8266 only has 512kb flash memory.  
**Note:** the 512kb version won't have the full mac vendors list.  

**2** Upload using the ESP8266 flash tool of your choice:  
	- [nodemcu-flasher](https://github.com/nodemcu/nodemcu-flasher) [Windows only]  
	- [esptool](https://github.com/espressif/esptool) [Windows, MacOS, Linux]

**That's all!**

Make sure your settings are correct for your board. Most boards come with 4mb flash and sometimes you have to hold the flash button down while plugging it in and hold it until the flashing process started.  

Also make sure you select the right com-port, the right upload size (mostly 4mb) and the correct .bin file.  

If it's not working, you can try using the Arduino as descriped below.

### Compiling the source with Arduino IDE

**1** Download the source code of this project.

**2** Install [Arduino](https://www.arduino.cc/en/Main/Software) and open it.

**3** Go to `File` > `Preferences`

**4** Add `http://arduino.esp8266.com/stable/package_esp8266com_index.json` to the Additional Boards Manager URLs. (source: https://github.com/esp8266/Arduino)
![screenshot of arduino, selecting the right version](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/Additional%20Boards%20Manager%20URLs.png)

**5** Go to `Tools` > `Board` > `Boards Manager`

**6** Type `esp8266`

**7** Select version `2.0.0` and click on `Install` (**recommended: v2.0.0 is used stability!**)

![screenshot of arduino, selecting the right version](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/arduino_screenshot_1.JPG)

**8** Go to `File` > `Preferences` in the Aurdino IDE

**9** Open the folder path under `More preferences can be edited directly in the file`

![screenshot of arduino, opening folder path](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/arduino_screenshot_2.JPG)

**10** Go to `packages` > `esp8266` > `hardware` > `esp8266` > `2.0.0` > `tools` > `sdk` > `include`

**11** Open `user_interface.h` with any text editor.(Recommended: Some auto indentent text editor)

**12** Scroll down to the end and before `#endif` add following lines:

```
typedef void (*freedom_outside_cb_t)(uint8 status);
int wifi_register_send_pkt_freedom_cb(freedom_outside_cb_t cb);
void wifi_unregister_send_pkt_freedom_cb(void);
int wifi_send_pkt_freedom(uint8 *buf, int len, bool sys_seq);
```  

![screenshot of notepad, copy paste the right code](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/notepad_screenshot_1.JPG)

**Save it!**  

**13** Open `eSP_Deauthenticator` > `eSP_Deauthenticator.ino` in Arduino from project folders.

**14** Connect your ESP to the System via USB type A.

**15** Select your ESP8266 board at `Tools` > `Board` and the right port at `Tools` > `Port`  
If no port shows up you may have to reinstall the drivers as suggested below.

	Windows: Go to Driver update from windows search, and update your chip drivers via right clicking on that Unknown Device > Properties > Update Drivers.
	
	Linux: (Tux mostly will not have problems)Under System Settings, double-click Additional Drivers. You'll then see that proprietary drivers are not in use. Click Activate to activate the driver and then, when prompted, enter your password and click Authenticate. Wait for the drivers to download and install. Then, click Close once the changes have been applied.

or you can try on your own.
	
**16** Depending on your board you may have to adjust the `Tools` > `Board` > `Flash Frequency` and the `Tools` > `Board` > `Flash Size`. I use a `160MHz` flash frequency and a `4M (3M SPIFFS)` flash size.

**17** `Compile` to test the code. If successfully compiled `continue`, else `try again new`.

**18** Upload!

**Your ESP8266 Deauthenticator is now fresh ready!**

## How to use it

First start your ESP8266 by plugging it in any power source.  

Scan for Wi-Fi networks and connect to `eSPd`. The password is `deauthenticator`.  

Once connected, you can open up your browser and go to `192.168.4.1`.  

You can now scan for networks...
![webinterface AP scanner](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/web_screenshot_1.JPG)

scan for client devices... 
![webinterface client scanner](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/web_screenshot_2.JPG)

Note: While scanning the ESP8266 will shut down its access point, so you may have to go to your settings and reconnect to the Wi-Fi network manually!

...and start different attacks.
![webinterface attack menu](https://github.com/mrsunnybourne/silkypirates/blob/master/screenshots/web_screenshot_3.JPG)

## License 

This software is licensed under the MIT License. See the [LICENSE](https://github.com/mrsunnybourne/silkypirates/blob/master/LICENSE) file for more details.  

`Thank you for visiting our repository, hope you like our project.`


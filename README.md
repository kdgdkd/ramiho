# ramiho - Raspberry Pi Midi Host manager

ramiho is a terminal front for Raspberry Pi that provides user friendly MIDI management tools and will turn your raspi into a rocking hub for MIDI musical instruments.   
For detailed instructions in Spanish, visit [kdg/dkd](http://edpanfleto.com/kdgdkd/).

## Features

ramiho is a script written in bash that was conceived to run on a headless Raspberry Pi.  
It builds on Neuma Studio's project [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). Their project will automatically connect MIDI device on the Raspberry Pi between themselves, which is all you need for simple setups like getting a MIDI controller to send information to a synthesizer. My needs are rather more complex, so I wrote ramiho mainly as a front for aconnect.   


ramiho's main features are:
- easy interface to manage MIDI connexions between devices  
- connexion testing tools (MIDI monitors and SendMidi)
- it allows to save current connexion configuration, and define alternative favorite setups that can be easily loaded
- all of ramiho's commands can be accessed using a numeric pad (only numbers and mathematical operators)
- it talks, it may be alive
  
ramiho generates a list of ports for the connected devices, and allows to operate (connect, disconnect, etc) referring to them only by their position in the list. This results in very lean commands; for example,   
- press **1** to see the list of ports and connexions  
- press **0** to connect all devices to each other  
- press **.** to close all connexions between ports  
- **+21** will send the MIDI Out signal from device number 2 to the MIDI In port of device number 1.  
- **-31** will disconnect device number 3 from device number 1   

## Dependencies
ramiho main functionalities require only ALSA; that's enough to use it for connexion management (aconnect) and MIDI monitor (aseqdump).  
For additional testing functionalities, ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) and [SendMidi](https://github.com/gbevin/ReceiveMIDI), by Geert Bevin. These provide a second monitor, and the send midi test. You may download them into ramiho's srmidi directory, or define their location in the fCONF function.   
For advanced connexion routings (like routing channels or notes) try [midish](https://midish.org/); ramiho embeds a front for it within the Favorite Connexions submenu.  
And if you wish ramiho to speak back, you will need a text-to-sound engine; I use [festival](http://festvox.org/festival/) (**sudo apt-get install festival**).


## Installation
Just copy these files under your user directory (typically under /home/pi/ramiho/). Make sure that the permissions are right, so ramiho is executable and can access favcnx files. I believe that is it.

## Customization
There are a number of elements that are expected to be changed in order to customize ramiho to the user's setup and preferences. These are organized in a rather clumsy way.  

**Locale**  
ramiho comes with Spanish and English interfaces. You are very wellcome if you want to create your own locales. Choose which to use below the locale functions (change fLOCALE_EN for fLOCALE_ES to turn ramiho's interface to Spanish).

**Favorite Connexions**  
ramiho allows to load preferred connexion configurations in batch, using the Favorite Connexions submenu. You should update the corresponding section of the code to have an easy way of loading alternative configurations for your setups.  

The Favorite Connexions menu currently uses three ways of saving connexions. 
- embedded in ramiho's code as aconnect commands (fSET_x functions)
- in one of three svdcnx files under favcnx directory; these are also used to save current configurations in form of lists of aconnect commands
- in one of two midishcnx files under favcnx directory, that allow for advanced routing configuration using midish  

You should then adapt the header (fSETX_HEADER) to display some description of the connexions you've set up. 


## Usage

The normal setup for ramiho would be on a headless Raspberry Pi, connected to a network with LAN or WIFI and operated through SSH... plus a number of MIDI devices connected to it's USB ports. If this is your case, you may consider taking the following steps:
- create a useful alias to acess ramiho (**nano /home/pi/.bash_aliases**); mine is **111**, so I can launch it with an external numeric pad 
- enable ssh connexions in your Raspberry Pi (**sudo raspi-config**)
- auto-connect your Raspberry Pi to WIFI network (**sudo nano /etc/wpa_supplicant/wpa_supplicant.conf**)
- define a static IP address (**sudo nano /etc/dhcpcd.conf**)
- on Raspberry Pi OS, set File System Overlay to be able to turn the device on and off with a power switch, without corrupting the SD card (**sudo raspi-config** then Performance/Overlay File System)  

Most of these steps are covered in Neuma Studio's [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html).   

### with terminal front
When you launch ramiho without arguments, it will open ramiho's terminal front. In English, it should look like this:    
![ramiho_terminal](https://edpanfleto.com/kdgdkd/assets/ramiho_term_en.png "ramiho terminal front" )  
Numbers 4, 7 and 8 will open submenus.  



### with command line
You can access the main features through the command line interface. To show all available commands
```bash
ramiho h
```  
![ramiho_cli](https://edpanfleto.com/kdgdkd/assets/ramiho_cli_en.png "ramiho cli" )  

### offline and headless
For some strange reason, ramiho was originally conceived to work from an offline Raspberry Pi, operated with an external numeric pad, providing audio feedback through a small speaker connected to the device's mini-jack.  
This is achieved through a text-to-sound engine, my preference is for festival.  
To configure a different tts engine and to activate sound by default, edit these lines in the fCONFIG function of the script:  
```bash
tts_on="festival --tts" 
tts=$tts_on
```  

You can also turn the sound on by sending **99** to ramiho.
If you are using an external [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png), you may want to deactivate BloqNum, so the buttons send functions, like arrows, END or PgDn, instead of numbers. Now, instead of commands, what lies below the numbers are the devices in the list. If you press 3 (PgDn), you'll hear the name of the third device in the list. You may dis/connect them normall using the operators.


## Epilogue
I have spent a significant amount of time coding ramiho, but I am very happy that it solves issues that we, as a band (**PUNKT25**), found when trying to properly configure our DAW-less techno setup. Setting up MIDI connexions between devices, or loading previously used sets of connexions, is now easily done with ramiho. My group-members, who wouldn't deal with aconnect on a command line, now operate ramiho on an ssh client on their own phones. This brings me a lot of joy. But I think I might be thrilled if I ever learn that this code is helping other people make music, so do not doubt getting in touch if you come to use it. 

## Contributing

I am stubborn, but I am no programmer. This script is likely to contain a significant amount of bugs, and most of the code could be polished and modified. So comments, suggestions, and pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.


## License

GNU General Public License v3.0

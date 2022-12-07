# ramiho - Raspberry Pi Midi Host manager

ramiho is a terminal front that provides user friendly MIDI management tools that will turn your Raspberry Pi into a rocking hub for MIDI musical inistruments.   
For detailed instructions in Spanish, visit [kdg/dkd](http://edpanfleto.com/kdgdkd/).

## Features

ramiho is a script written in bash that was conceived to run on a headless Raspberry Pi.  
ramiho generates a list of connected devices and allows to operate (connect, disconnect, etc) referring to them only by their position in the list. This allows for very lean commands; for example,   
- **+21** will send the MIDI Out signal from device number 2 to the MIDI In port of device number 1.  
- **-31** will disconnect device number 3 from device number 1   
- press 0 to connect all devices to each other  
- press . to close all existing connexions

ramiho's main features are:
- easy interface to manage MIDI connexions between devices  
- connexion testing tools (MIDI monitors and SendMidi)
- it allows to save current connexion configuration, and define alternative favorite setups that can be easily loaded
- all of ramiho's commands can be accessed using a numeric pad (only numbers and mathematical operators)
- it talks, it may be alive



## Dependencies
ramiho main functionalities require only ALSA; that's enough to use it for connexion management (aconnect) and MIDI monitor (aseqdump).  
For additional testing functionalities, ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) and [SendMidi](https://github.com/gbevin/ReceiveMIDI), by Geert Bevin. These provide a second monitor, and the send midi test. You may download them into the ramiho's srmidi directory, or define their location in the fCONF function.   
For advanced connexion routings you may use [midish](https://midish.org/); ramiho embeds a front for it.  
And if you wish ramiho to speak back, you will need a text-to-sound engine; I use [festival](http://festvox.org/festival/) (**sudo apt-get install festival**).


## Installation
Just copy these files under your user directory (typically under /home/pi/ramiho/). Make sure that the permissions are right, so ramiho is executable and can access favcnx files. That is it.

## Customization
There are a number of elements that are expected to be changed in order to customize ramiho to the user's setup and preferences. These are organized in a rather clumsy way.  

**Locale**  
ramiho comes Spanish and English interfaces. You are very wellcome if you want to create your own locales. Choose which to use below the locale functions (change fLOCALE_EN for fLOCALE_ES to turn ramiho's interface to Spanish).

**Favorite Connexions**  
ramiho allows to load preferred connexion configurations in batch, using the Favorite Connexions submenu. You should update the corresponding section of the code to have an easy way of loading alternative configurations for your setups.  

The Favorite Connexions menu currently uses three ways of saving connexions. 
- embedded in ramiho's code as aconnect commands (fSET_x functions)
- in one of three svdcnx files under favcnx directory; these can also be used to save current configurations in form of aconnect commands
- in one of three midishcnx under favcnx directory, that allow for advanced routing configuration using midish
You should then adapt the header (fSETX_HEADER) to display some description of the connexions to choose from.

Obviously, you are invited to modify whatever itches you most, please share!

## Usage

The normal setup for ramiho would be on a headless Raspberry Pi, connected to a network with LAN or WIFI and operated through SSH, and a number of musical devices connected to it's USB ports. If this is your case, you may consider taking the following steps:
- habilitate ssh connexion to your Raspberry Pi (**sudo raspi-config**).
- auto-connect to WIFI network (**sudo nano /etc/wpa_supplicant/wpa_supplicant.conf**)
- define a static IP address for the Raspberry Pi (**nano /etc/dhcpcd.conf**)
- set File System Overlay to be able to turn the device on and off with a switch without corrupting the SD card(**sudo raspi-config** then Performance/Overlay File System)  

All of these steps are properly documented and out of this README's scope.

### with terminal front
When you launch ramiho without arguments, ramiho's terminal front will open. It should look like this:  
![ramiho_terminal](https://edpanfleto.com/kdgdkd/assets/ramiho_term_en.png "ramiho terminal front" )  
Numbers 4, 7 and 8 will open submenus.  



### with command line
You can access the main features through the command line interface. To show all available commands, enter 
```bash
ramiho h
```  
![ramiho_cli](https://edpanfleto.com/kdgdkd/assets/ramiho_cli_en.png "ramiho cli" )  

### offline and headless
For some strange reasons, ramiho was originally conceived to work from an offline Raspberry Pi, operated with an external numeric pad, and receiving audio feedback through a small speaker connected to the device's mini-jack.  
This is achieved through a text-to-sound engine, my preference is for festival.  
To configure a different tts engine and to activate sound by default, edit these lines in the fCONFIG function of the script:  
```bash
tts_on="festival --tts" 
tts=$tts_on
```  

If you are using an external numberic pad, this mode works better deactivating BloqNum (so the buttons send functions, like END or PgDn, instead of numbers). This way, when you press one of the buttons (1-9) you'll hear the name of the device on that position. 


## Epilogue
I have spent a significant amount of time in this, and I am very happy that it works for me. I think I might be thrilled if I ever learn that this code is helping other people make music, so do not doubt getting in touch. 

## Contributing

I am stubborn, but I am no programmer. This script very likely contains a significant amount of bugs, and most of the code could be polished and modified. So pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.


## License

GNU General Public License v3.0
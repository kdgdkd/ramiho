# ramiho - Raspberry Pi Midi Host manager

ramiho is a terminal interface managing MIDI connexions on your Raspberry Pi.   
For detailed instructions in Spanish, visit [kdg/dkd](http://edpanfleto.com/kdgdkd/).

## Features

ramiho is a script written in bash conceived to run on a headless Raspberry Pi.  
It builds on Neuma Studio's project [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). Their project will automatically connect all MIDI devices on the Raspberry Pi between themselves, which is all you need for simple setups like getting a MIDI controller to send information to a synthesizer. For more complex setups, like when you add more controllers, synths, or an external clock, you need to resort to aconnect. I wrote ramiho initially as an accesible front-end for aconnect to sort MIDI routing issues.   


ramiho's main features are:
- simple interface to manage MIDI connexions between devices  
- connexion testing tools (MIDI monitors and SendMidi)
- it allows to save current connexion configuration, and define alternative favorite setups that can be easily loaded
- all of ramiho's commands can be accessed using a numeric pad (only numbers and mathematical operators)
- it talks, it may be alive
  
ramiho generates a list of available MIDI ports from the connected devices, and allows to operate (connect, disconnect, etc) referring only to their position in the list. This results in very lean commands; for example,   
- press **1** to see the list of ports and connexions  
- **+21** will send the MIDI Out signal from device number 2 to the MIDI In port of device number 1.  
- **-31** will disconnect device number 3 from device number 1   

## Dependencies
ramiho main functionalities require only ALSA; that's enough to use it for connexion management and MIDI monitor (using aconnect and aseqdump).  
ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI), by Geert Bevin, as a monitor than can be filtered by type of data. You may download it into ramiho's srmidi directory, or define it's location in the fCONF function.   

And if you wish ramiho to speak back, you will need a text-to-sound engine; I use [festival](http://festvox.org/festival/) (**sudo apt-get install festival**).


## Installation
Copy the files and directories in this repository to your user directory (typically /home/pi/ramiho/). Make sure that the permissions are right, so ramiho is executable and can access favcnx files. You may also download the programs mentioned on the previous section. I believe that should be it.

## Customization
There are a number of elements that are expected to be changed in order to customize ramiho to the user's setup and preferences. I fear these are organized in a rather clumsy way. To edit ramiho  

```bash
nano /home/pi/ramiho/ramiho
``` 

**Locale**  
ramiho comes with Spanish and English interfaces. ramiho would like to speak your language, so please consider creating new locales. Choose which to use below the locale functions (change fLOCALE_EN for fLOCALE_ES to turn ramiho's interface to Spanish).

**Favorite Connexions**  
ramiho allows to load preferred connexion configurations in batch, using the Favorite Connexions submenu. You should customize this section of the code to have an easy way of loading your own favorite setup configurations.  

The Favorite Connexions menu currently uses three ways of saving connexions. 
- embedded in ramiho's code as aconnect commands (fSET_x functions)
- in one of three svdcnx files under favcnx directory; these are also used to save current configurations in form of lists of aconnect commands
- in one of two midishcnx files under favcnx directory, that allow for advanced routing configuration using midish  

You should then adapt the header of the menu (fSETX_HEADER) to display some description of the connexions you've set up. 


## Usage

The normal use of ramiho would be on a headless Raspberry Pi, connected to a WIFI network and operated through an SSH client... plus a number of MIDI devices connected to it's USB ports. If this is your case, you may consider taking the following steps:
- create a useful alias to acess ramiho (**nano /home/pi/.bash_aliases**); mine is **111**, so I can launch it with an external numeric pad 
- enable ssh connexions in your Raspberry Pi (**sudo raspi-config**)
- auto-connect your Raspberry Pi to WIFI network (**sudo nano /etc/wpa_supplicant/wpa_supplicant.conf**)
- define a static IP address (**sudo nano /etc/dhcpcd.conf**)
- on Raspberry Pi OS, set File System Overlay to be able to turn the device on and off with a power switch, without corrupting the SD card (**sudo raspi-config** then Performance/Overlay File System)  

Most of these steps are covered in Neuma Studio's [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html).   

### with terminal interface
When you launch ramiho without arguments, it will open ramiho's terminal interface. In English, it should look similar to this:    
![ramiho_terminal](https://edpanfleto.com/kdgdkd/assets/ramiho_term_en.png "ramiho terminal front" )  
Numbers 4, 7 and 8 will open submenus.  



### with command line
You can access the main features through the command line interface. To show all available commands type
```bash
ramiho h
```  
![ramiho_cli](https://edpanfleto.com/kdgdkd/assets/ramiho_cli_en.png "ramiho cli" )  

### offline and headless
For some strange reason, ramiho was originally conceived to work from an offline Raspberry Pi, operated with an external numeric pad, providing audio feedback through a small speaker connected to the device's mini-jack.  
This is achieved with a text-to-sound engine, my preference is for festival. Text will be piped into the tts engine ($tts_on).   
To configure a different tts engine and to activate sound by default, edit these lines in the fCONFIG function of the script:  
```bash
tts_on="festival --tts" 
tts=$tts_on
```  

You can also turn the sound on and off by sending **99** to ramiho.   
If you are using an external [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png), you may want to deactivate BloqNum, so the buttons send functions, like arrows, END or PgDn, instead of numbers. Now, instead of commands, what lies below the numbers are the devices in the list. If you press 3 (PgDn), you'll hear the name of the third device in the list. You may dis/connect them normall using the operators.


## Epilogue
I have spent a significant amount of time writing ramiho, but I am very happy that it solves issues that we, as a band (**PUNKT25**), found when trying to properly configure our DAW-less techno setup. Setting up MIDI connexions between devices, or loading previously used sets of connexions, is now easily done with ramiho. My group-members, who wouldn't care about aconnecting anything on a command line, now operate ramiho on ssh clients on their own phones. This brings me a lot of joy. But I think I might be thrilled if I ever learn that this code is helping other people make music, so do not doubt getting in touch if you come to use it. 

## Contributing

I am stubborn, but I am no programmer. And I am a newbie on GitHub and sharing code. This script is likely to contain a significant amount of bugs, and most of it could be polished and modified. Comments, suggestions, and pull requests are welcome. 


## License

GNU General Public License v3.0

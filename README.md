# ramiho - Raspberry Pi Midi Host manager

ramiho is a terminal interface managing MIDI connexions on your Raspberry Pi.   
For detailed instructions in Spanish, visit [kdg/dkd](http://edpanfleto.com/kdgdkd/).

## Features

ramiho is a script written in bash conceived to manage MIDI connexions on a headless Raspberry Pi.   
It builds on Neuma Studio's project [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). Their project will automatically connect all MIDI devices on the Raspberry Pi between themselves, which is all you need for simple setups like getting a MIDI controller to send information to a synthesizer. For more complex setups, like when you add more controllers, synths, or an external clock, you need to resort to aconnect.   
Initially, I wrote ramiho as an accesible front-end for aconnect to sort MIDI routing issues.   


ramiho's main features are:
- simple, lightweight interface to manage MIDI connexions between devices
- connexion testing tools (MIDI monitors and SendMidi)
- it allows to save current connexion configuration, and define alternative favorite setups that can be easily loaded
- all operations are accessible by using only the [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png) (numbers and mathematical operators)  
- it talks, it may be alive
  
ramiho generates a list of available MIDI ports from the connected devices, and allows to operate (connect, disconnect, etc) referring only to their position in the list. This results in very lean commands; for example,   
- press **1** to see the list of ports and connexions  
- [**+12**](https://edpanfleto.com/kdgdkd/git/ramiho_connect.png) will send the MIDI Out signal from device number 1 to the MIDI In port of device number 2.  
- **-3\*** will disconnect device number 3 from every other device (* is a wildcard)   

## Dependencies
ramiho is written in bash, and only requires ALSA for MIDI connexion management. I guess that ramiho should work on early Raspberry Pi's and other Linux devices running ALSA. But don't take my word.    
For additional testing functionalities, ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) and [SendMidi](https://github.com/gbevin/ReceiveMIDI), both by Geert Bevin. These tools provide a filtered monitor and the send midi test. The default location for these executables is ramiho's srmidi directory, but you may define any other location (or alias) in the fCONF function.   
For advanced connexion configurations (like routing MIDI channels) ramiho uses [Midish](https://midish.org), by Alexandre Ratchov.   
And if you wish ramiho to speak back, you will need a text-to-sound engine; I use [festival](http://festvox.org/festival/) (**sudo apt-get install festival**).


## Installation
Copy the files and directories in this repository to your user directory (typically /home/pi/ramiho/). Make sure that the permissions are right, so ramiho is executable and can access favcnx files. You may also download the programs mentioned on the previous section. I believe that should be it.

**Setting-up a headless Raspberry Pi**  
The standard setup for ramiho would be on a headless Raspberry Pi, connected to a WIFI network and operated through an SSH client... and with a number of MIDI devices connected to it's USB ports. If this is your case, you may consider taking the following steps:
- create a useful alias to acess ramiho (**nano /home/pi/.bash_aliases**); mine is **111**, so I can launch it with an external numeric pad 
- enable ssh connexions in your Raspberry Pi (**sudo raspi-config**)
- auto-connect your Raspberry Pi to WIFI network (**sudo nano /etc/wpa_supplicant/wpa_supplicant.conf**)
- define a static IP address (**sudo nano /etc/dhcpcd.conf**)
- on Raspberry Pi OS, set File System Overlay to be able to turn the device on and off with a power switch, without corrupting the SD card (**sudo raspi-config** then Performance/Overlay File System)  

Most of these steps are covered in Neuma Studio's [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). If you run ramiho on a desktop or laptop you could do very well without all of this fuzz.     


## Customization
There are a number of elements that are expected to be customized to the user's own preferences. Just open ramiho on a text or code editor.   


**Locale**  
ramiho comes with Spanish and English interfaces. ramiho would like to speak your language, so please consider creating new locales. Choose which to use within the fCONF function (change fLOCALE_EN for fLOCALE_ES to turn the interface to Spanish).

**Favorite Connexions**  
ramiho allows to load preferred connexion configurations in batch, using the Favorite Connexions submenu. You should customize this section of the code to have an easy way of loading your own favorite setup configurations.  

The Favorite Connexions menu currently uses three ways of saving connexions: 
- embedded in ramiho's code as a list of aconnect commands (fSET_x functions)
- in one of three svdcnx files under favcnx directory; these are also used to save current configurations in form of lists of aconnect commands, so they may get overwritten
- in one of two midishcnx files under favcnx directory, that allow for advanced routing configuration using midish  

You may edit these files and functions, and adapt the header of the menu (fSETX_HEADER and fSET_ACT) to display some description of the connexions you've set up. 


## Usage


### with terminal interface
When ramiho is launched without arguments, it will load ramiho's terminal interface. This is the main interface for ramiho. You will see the Help header with the list of available commands, and the list of MIDI ports and connexions. In English, it should look similar to this:    
<img src="https://edpanfleto.com/kdgdkd/git/ramiho_terminal.png" alt="ramiho_terminal" height="500"/>  

There is already one connexion set, but you'd like to connect device number 2 to device number 4; enter **+24** [like this](https://edpanfleto.com/kdgdkd/git/ramiho_connect.png "ramiho connect" ).   
If you are happy with the current connexions and want to save them for later use, enter **0** to open the [Favorite Connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnx.png "Favorite Connexions" ) submenu; mine is in Spanish.  
Now enter **3** to [save connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnxsave.png "Save Connexions" ) into file svdcnx3.  
Next time you enter ramiho, you'll just need to go to the Favorite Connexions submenu, and enter **3** to [load connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnxload.png "Load Connexions" ) file svdcnx3.

### with command line
You can access all of ramiho's features through the command line interface, by adding arguments after the ramiho command. Commands mirror the terminal interface's; to show all available commands type
```bash
ramiho h
```  
and ramiho will print [this](https://edpanfleto.com/kdgdkd/git/ramiho_cli_en.png).   
Arguments will open ramiho's functionalities, just as in the terminal interface; for example, **ramiho +** will open the dialog to create a new connexion.  
You can open and close connexions from the cli with single commands. If you follow **ramiho +** with two numbers, say **ramiho +24**, ramiho will connect port number 2 to port number 4 without opening any dialog. In a similar way, **ramiho -24** would close the previous connexion.   
You can also load your Favorite Connexions with a single command. Enter **ramiho \*n**, with n being a number from 1 to 9, to load presets as defined in fSET_ACT.


### using * when connecting / disconnecting
ramiho accepts using **\*** as a symbol for "every other device". On the terminal   
- +2* will connect device number 2 to every other device   
- +*3 will connect every other device to device number 3
- -*4 will disconnect every other device from device number 4   



### offline and headless
For some strange reason, ramiho was originally conceived to work from an offline Raspberry Pi, operated with an external numeric pad, providing audio feedback through a small speaker connected to the mini-jack output.  
To work this out, you will need a text-to-sound engine; there's espeak, my preference is for festival. Text will be piped into the tts engine ($tts_on). Send **99** to turn sound on and off.      
To configure a different tts engine and to activate sound by default, edit these lines in the script's fCONFIG function:  
```bash
tts_on="festival --tts" 
tts=$tts_on
```  

If you are using an external [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png), you may want to deactivate BloqNum, so the buttons send functions, like arrows, END or PgDn, instead of numbers. Now buttons 1 to 9 represent devices, and if you press button 3 (or rather, PgDn), you'll hear the name of the third device in the list. Operators dealing with connexions work just the same. 

### testing tools
Let's say you connect two MIDI devices with ramiho, but it does not work, the information does not seem to flow, you get no sound. As always, this begs the question "what MIDI channel are you using?"  
ramiho proposes two tools that should help testing input and output MIDI ports on your devices:   
- MIDI monitor - it tests the sending device. It will print MIDI information (like the channel) coming from the sending ports. The first monitor uses ALSA's aseqdump; for the filtered monitors you'll need [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI). In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_monitor.png) ramiho monitors the first port for incoming Control Change signals, and prints the result.      
- Send MIDI - it tests the receiving device. Use ramiho's front-end for [SendMidi](https://github.com/gbevin/ReceiveMIDI) to send notes directly from the Raspberry Pi to your receiving device. In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_sendmidi.png) ramiho sends note 48, on channel 3, to the fourth device in the list (the 'sound' port of a synth).    

If these tests fail, check the cables and whether the hardware is properly configured to send/receive MIDI. Then, you may want to try with Midish.  

### using Midish   
[Midish](https://midish.org/) is a powerful command-line MIDI sequencer/filter by Alexandre Ratchov. It provides advanced MIDI manipulation tools (routing channels, remapping CCs, transposing...), and it also records and plays MIDI. Within the Favorite Connexions menu, ramiho provides a front-end for Midish's MIDI routing, and two files with pre-loaded connexions and filters.   
The front-end allows defining connexions not only between ports, but also between channels. You can redirect the MIDI signal coming from one port on a particular channel to any other port, on any MIDI channel (well, not exactly: only channels 1 to 10 seem to work). For example, you could send the output from a MIDI keyboard on channel 1 to a synth that is reading MIDI channel 4 AND to a second synth reading channel 5, in order to have a bass and a sub-bass playing the same notes.   
You can define more complex connexions and filters within the midishcnx files in ramiho's favcnx directory. They will be available for loading within ramiho's Favorite Connexions menu (lauched from the fMSH_x functions).   

## Epilogue
I have spent a significant amount of time writing ramiho, but I am very happy that it solves issues that we, as a band (**PUNKT25**), found when trying to properly configure our DAW-less techno setup. Setting up MIDI connexions between devices, or loading previously used sets of connexions, is now easily done with ramiho. My group-members, who wouldn't care about aconnecting anything on a command line, now operate ramiho on ssh clients on their own phones. This brings me a lot of joy. But I think I might be thrilled if I ever learn that this code is helping other people make music, so do not doubt getting in touch if you come to use it. 

## Contributing

I am stubborn, but I am no programmer. And I am a newbie on GitHub and sharing code. This script is likely to contain a significant amount of bugs, and most of it could be polished and modified (do I really need 1,000 variables?). Comments, suggestions, and pull requests are welcome. 


## License

GNU General Public License v3.0

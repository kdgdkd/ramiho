# ramiho - Midi Host manager

ramiho is a command-line interface managing ALSA MIDI connexions.  

[Inpired by Neuma Studio's project [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). Neuma's project will automatically connect all MIDI devices on the Raspberry Pi between themselves, which is all you need for simple setups like getting a MIDI keyboard to send notes to a virtual or hardware synthesizer.] 

## Features

ramiho's main features are:
- simple, lightweight cli using ALSA to manage MIDI connexions between devices
- connexion testing tools (MIDI monitors and SendMidi)
- easily save and load your favorite connexion setups  
- simple interface for midish, allowing for MIDI routings per channel, merging, splitting... 
- all operations are accessible using only the [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png) (numbers and mathematical operators)  
- it talks, it may be alive... (useful if you are using a headless computer)  
  

## Dependencies
ramiho is written in bash, and should work on any Linux distributions using ALSA.   

[Optional]  
For additional testing functionalities, ramiho uses two applications by Geert Bevin. [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) is used as for a filtered monitor, and [SendMidi](https://github.com/gbevin/ReceiveMIDI) to test output connexions. The default location for these executables is ramiho's srmidi directory, but you may define any other location (or alias) in the fCONF function.   
For advanced connexion configurations (like routing, splitting and merging MIDI channels) ramiho uses [midish](https://midish.org), by Alexandre Ratchov.   
ramiho speaks, but you will need a text-to-sound engine. It works with [festival](http://festvox.org/festival/), and probably with [eSpeak](https://espeak.sourceforge.net/).


## Installation
Copy the files and directories in this repository to your user directory (git clone https://github.com/kdgdkd/ramiho). Make sure that the permissions are right, ramiho is executable and can access favcnx files. You may want to define an alias, and install the optional software from the previous section.   


## Usage

### with console interface
When ramiho is run without arguments, it will load ramiho's console interface. In English, it should look similar to this:    
<img src="https://edpanfleto.com/kdgdkd/git/ramiho_terminal.png" alt="ramiho_terminal" height="500"/>  
  
The top part is the Help header showing the list of available commands, and below is the list of current MIDI ports and connexions. Commands entered in the prompt will open new menus (favorite connexions, debugging), start connexion dialogs or provide information. 

### setting up connexions 
ramiho generates a numbered list of available MIDI ports from connected devices, and allows to use ALSA's aconnect with very simple commands. 
- press **1** to see the list of ports and connexions   
- [**+14**](https://edpanfleto.com/kdgdkd/git/ramiho_connect.png) to connect  port number 1 to port number 4   
<img src="https://edpanfleto.com/kdgdkd/git/ramiho_connect.png" alt="ramiho_connect"  height="250"/> 
- press **4** to connect every port to any other port 
- press **.** (dot) to break all existing connexions, or **-12** to break only the connexion from port number 1 to port number 2


#### using * 
ramiho accepts using **\*** as a symbol for "every other port/device" when connecting & disconnecting. On the terminal   
- **+2\*** will connect port number 2 to every other port   
- **+\*3** will connect every other port to port number 3
- **-3\*** will close connexions from port number 3 to any other port



### testing tools
Let's say you connect two MIDI devices with ramiho, but it does not work, the information does not seem to flow, you get no sound. 8 times out of 10 you are not on the same channel.  
ramiho proposes two tools to help testing input and output MIDI ports on your devices:   
- MIDI monitor - it tests the sending device. It will print MIDI information (like the channel) coming from the sending ports. The first monitor uses ALSA's aseqdump; for the filtered monitors you'll need [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI). In the [example](https://edpanfleto.com/kdgdkd/git/ramiho_monitor.png) ramiho monitors the first port for incoming Control Change signals, and prints the result.      
- Send MIDI - it tests the receiving device. Use ramiho's interface for [SendMidi](https://github.com/gbevin/ReceiveMIDI) to send notes directly from the Raspberry Pi to your receiving device. In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_sendmidi.png) ramiho sends note 48, on channel 3, to the fourth device in the list (the 'sound' port of a synth).    

 

### using Midish   
[Midish](https://midish.org/) is a powerful command-line MIDI sequencer/filter by Alexandre Ratchov. It provides advanced MIDI manipulation tools (routing channels, remapping CCs, transposing...), and it also records and plays MIDI. The Midish menu in ramiho provides a simple front-end for Midish's MIDI routing, and saving/loading of existing or customized connexions.    
The midish interface allows defining connexions not only between ports, but also between channels. You can redirect the MIDI signal coming from one port on a particular channel to any other port, on any MIDI channel. And you can choose to transpose the notes! For example, you could send the output from a MIDI keyboard on channel 1 to a synth that is reading MIDI channel 4 AND to a second synth reading channel 5 a full octave below, so you would have a bass and a sub-bass, like in [this example](https://edpanfleto.com/kdgdkd/git/ramiho_midishrouting.png).     
You can work on the last connexion that you defined or loaded, adding routings, velocity adjustments, send program change signal... And when you are happy, you can save the connexion so it will be easily loaded in the future. Or you can define more complex connexions and filters within the mdshcnx files in ramiho's favcnx directory. All of these files are directly accessible from the Midish menu.  


### using ramiho from the  command line
You can also access all of ramiho's commands from the CLI, without opening the console interface, by adding arguments after the ramiho command.  
Commands mirror the console interface's; to show all available commands type
```bash
ramiho 9
```  
and ramiho will print [this](https://edpanfleto.com/kdgdkd/git/ramiho_cli_en.png).   
Arguments can be used to access ramiho's functionalities directly; for example, **ramiho 1** will show the current devices and connexions, and **ramiho +** will open the dialog to create a new connexion.  
You can open and close connexions from the cli with single commands. If you follow **ramiho +** with two numbers, say **ramiho +24**, ramiho will connect port number 2 to port number 4 without opening any dialog. In a similar way, **ramiho -24** will close the previous connexion.   
You can combine commands into bash aliases, or simply load one of your Favorite Connexions: enter **ramiho \*n**, with n being a number from 1 to 9, to load presets as defined in fFAV_ACT (check Customization section).

### connexions autosave/autoload
When you exit ramiho, the current alsa or midish connexions are saved in the favorite connexions folder. When opening ramiho, the program will automatically try to load those same connexions. This would come very handy if you wanted to work with a...


### RaspberryPi MIDI HOST
ramiho, as it's name only partially suggests, was originally conceived to run on a headless, offline Raspberry Pi 3b, operated with an external numeric pad, and providing audio feedback through a small speaker connected to the mini-jack output. That is why all the commands require only numbers and math operators, and why it speaks.  


Running ramiho on a headless Raspberry Pi will turn it into a smart USB MIDI HOST. You would need to connect the Raspberry Pi to the network and enable SSH so you can access the CLI or ramiho's console interface (sorry what¿) from another device.


#### running offline

Or you could also branch a speaker and run it offline!    
The audio feedback is generated by a text-to-sound engine(I like festival). 
 To configure a different tts engine and to activate sound by default, edit these lines in the script's fCONFIG function:  
```bash
tts="festival --tts" 
soundon='true'
```  
You can also turn sound on and off by sending **98** in the main interface.   

If you are using an external [numeric pad](http://edpanfleto.com/kdgdkd/git/numpad.png), clic BloqNum to enter 'device mode'. The buttons will send functions (like arrows, END or PgDn) instead of numbers, and ramiho will read aloud the name por ports 1 to 9 (the number in the same button). So if you press AvPag button (number 3), you'll hear the name of the third device in the list. You can use operators, like + and -, to connect the ports in this mode guided only by ramiho's voice. 


## Customization

ramiho works as an ALSA connexion interface and MIDI monitor out of the box, without any tralala. But there are a number of defaults that can be customized to the user's own preferences... which is half of the fun of using open-source software! Some of the changes require that you open ramiho on a text editor and look for the relevant function; it shouldn't be difficult to figure out (otherwise, please shout).   


**Locale**  
ramiho comes with Spanish and English interfaces. Choose which locale to use by default within the fCONF function (change fLOCALE_EN for fLOCALE_ES to turn the interface to Spanish). You can also switch between them by entering **97** in ramiho's interface. 

**Favorite Connexions**  
In ramiho you can save your preferred connexion setups, using the [Favorite Connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnx.png "Favorite Connexions" ) & the midish submenus. 

ramiho uses four ways of saving connexions: 
ALSA connexions
- embedded in ramiho's code under three fFAV_x functions, as lists of aconnect commands
- aconsvd files under favcnx directory, as lists of aconnect commands; these files are also used to save current aconnect configurations and may get overwritten
midish connexions
- mdshcnx files under favcnx directory, that allow for advanced routing configuration using midish   
- mdshsvd files under favcnx directory, as midish shell files; there are used to save current midish configurations and may get overwritten   

After saving your preferred connexions on any of these ways, you may want adapt the header of the menu (under fFAV_HEADER) with a description of the connexions you've set up, and link them with commands (fFAV_ACT). 



## Conclusion
ramiho is done with love, but little skill. It is a tool that I did for myself, and has been evolving for years depending on the moment's needs or dreams. It works well, but it probably tries to do too many things, and the code is a bit of a mess. Any comments, suggestions, bug reports and pull requests are welcome.  


## License

GNU General Public License v3.0

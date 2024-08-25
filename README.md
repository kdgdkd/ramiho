# ramiho - Raspberry Pi Midi Host manager

ramiho is a command-line interface helping with ALSA MIDI connection management.  

[Inpired by Neuma Studio's project [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). Neuma's project will automatically connect all MIDI devices on the Raspberry Pi between themselves, which is all you need for simple setups like getting a MIDI keyboard to send notes to a virtual or hardware synthesizer.] 

## Features

ramiho's main features are:
- simple, lightweight cli to manage MIDI connexions between devices
- connexion testing tools (MIDI monitors and SendMidi)
- save connection setups, and define alternative favorite setups that can be easily loaded
- when opening ramiho, it will load the connexions from your last session (and should work if the same MIDI devices are connected)  
- interface for midish connexions, allowing for MIDI routings per channel, merging, splitting... 
- all operations are accessible using only the [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png) (numbers and mathematical operators)  
- it talks, it may be alive... which may be useful if you are using a headless computer
  

## Dependencies
ramiho is written in bash, and should work on any Linux distributions using ALSA.   

[Optional]
For additional testing functionalities, ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) and [SendMidi](https://github.com/gbevin/ReceiveMIDI), both by Geert Bevin. These tools provide a filtered monitor and the send midi test. The default location for these executables is ramiho's srmidi directory, but you may define any other location (or alias) in the fCONF function.   
For advanced connexion configurations (like routing and and splitting and merging MIDI channels) ramiho uses [midish](https://midish.org), by Alexandre Ratchov.   
And if you wish ramiho to speak back, you will need a text-to-sound engine; I use [festival](http://festvox.org/festival/) (**sudo apt-get install festival**).


## Installation
Copy the files and directories in this repository to your user directory (git clone https://github.com/kdgdkd/ramiho). Make sure that the permissions are right, ramiho is executable and can access favcnx files. You may also install the optional software from the previous section.   



## Customization
ramiho works as an ALSA connexion interface and MIDI monitor out of the box, without any tralala. But there are a number of defaults that can be customized to the user's own preferences... which is half of the fun of using open-source software! Some of the changes require that you open ramiho on a text editor and look for the relevant function; it shouldn't be difficult to figure out (otherwise, please shout).   


**Locale**  
ramiho comes with Spanish and English interfaces. Choose which locale to use by default within the fCONF function (change fLOCALE_EN for fLOCALE_ES to turn the interface to Spanish). You can also switch between them by entering **97** in ramiho interface. 

**Favorite Connexions**  
In ramiho you can save your preferred connexion setups, using the [Favorite Connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnx.png "Favorite Connexions" ) & the midish submenus. 

ramiho uses four ways of saving connexions: 
ALSA connexions
- embedded in ramiho's code under three fFAV_x functions, as lists of aconnect commands
- aconsvd files under favcnx directory, as lists of aconnect commands; these files are also used to save current aconnect configurations, so they may get overwritten
midish connexions
- mdshcnx files under favcnx directory, that allow for advanced routing configuration using midish   
- mdshsvd files under favcnx directory, as midish shell files; there are used to save current midish configurations and may get overwritten   

After saving your preferred connexions on any of these ways, you may want adapt the header of the menu (under fFAV_HEADER) with a description of the connexions you've set up, and link them with commands (fFAV_ACT). 


## Usage

ramiho generates a dynamic list of available MIDI ports from connected devices, and allows to operate (connect, disconnect, etc) referring only to their position in the list. This results in very lean commands; for example,   
- press **1** to see the list of ports and connexions   
- [**+14**](https://edpanfleto.com/kdgdkd/git/ramiho_connect.png) will send the MIDI signal from port number 1 to port number 4    

#### using * 
ramiho accepts using **\*** as a symbol for "every other port/device" when connecting & disconnecting. On the terminal   
- **+2\*** will connect port number 2 to every other port   
- **+\*3** will connect every other port to port number 3
- **-3\*** will close connexions from port number 3 to any other port

### with terminal interface
When ramiho is launched without arguments, it will load ramiho's terminal interface. This is the main interface for ramiho. You will see the Help header with the list of available commands, and the list of MIDI ports and connexions. In English, it should look similar to this:    
<img src="https://edpanfleto.com/kdgdkd/git/ramiho_terminal.png" alt="ramiho_terminal" height="500"/>  



### with command line
You can also access all of ramiho's features through the command line interface, by adding arguments after the ramiho command. Commands mirror the terminal interface's; to show all available commands type
```bash
ramiho 9
```  
and ramiho will print [this](https://edpanfleto.com/kdgdkd/git/ramiho_cli_en.png).   
Arguments can be used to access ramiho's functionalities directly; for example, **ramiho +** will open the dialog to create a new connexion.  
You can open and close connexions from the cli with single commands. If you follow **ramiho +** with two numbers, say **ramiho +24**, ramiho will connect port number 2 to port number 4 without opening any dialog. In a similar way, **ramiho -24** would close the previous connexion.   
You can also load your Favorite Connexions with a single command. Enter **ramiho \*n**, with n being a number from 1 to 9, to load presets as defined in fFAV_ACT (check Customization above).


### offline and headless
ramiho was originally conceived to work from a headless, offline Raspberry Pi, working as a USB MIDI host, operated with an external numeric pad, and providing audio feedback through a small speaker connected to the mini-jack output. That is why all the functionalities of ramiho can be accessed by using only numbers and math operators, and why it speaks. The "visual" command line interface is best if your computer has a screen, or if it is online and can be accessed with SSH (for example). But otherwise, branch a speaker and...    

First, you will need a text-to-sound engine; there's espeak, I like festival. Text will be piped into the tts engine ($tts variable in fCONF_INIT). To configure a different tts engine and to activate sound by default, edit these lines in the script's fCONFIG function:  
```bash
tts="festival --tts" 
soundon='true'
```  
You can also turn sound on and off by sending **98** in the main interface.   

If you are using an external [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png), you may want to deactivate BloqNum, so the buttons send functions (like arrows, END or PgDn) instead of numbers. For ramiho, these functions represent devices 1 to 9 (the number in the same button); so if you press PgDn button (number 3), you'll hear the name of the third device in the list. Operators dealing with connexions, like + and - work just the same. 

### testing tools
Let's say you connect two MIDI devices with ramiho, but it does not work, the information does not seem to flow, you get no sound. As always, this begs the question "what MIDI channel are you using?"  
ramiho proposes two tools to help testing input and output MIDI ports on your devices:   
- MIDI monitor - it tests the sending device. It will print MIDI information (like the channel) coming from the sending ports. The first monitor uses ALSA's aseqdump; for the filtered monitors you'll need [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI). In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_monitor.png) ramiho monitors the first port for incoming Control Change signals, and prints the result.      
- Send MIDI - it tests the receiving device. Use ramiho's front-end for [SendMidi](https://github.com/gbevin/ReceiveMIDI) to send notes directly from the Raspberry Pi to your receiving device. In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_sendmidi.png) ramiho sends note 48, on channel 3, to the fourth device in the list (the 'sound' port of a synth).    

If these tests fail, check the cables and whether the hardware is properly configured to send/receive MIDI. Then, you may want to try with Midish.  

### using Midish   
[Midish](https://midish.org/) is a powerful command-line MIDI sequencer/filter by Alexandre Ratchov. It provides advanced MIDI manipulation tools (routing channels, remapping CCs, transposing...), and it also records and plays MIDI. The Midish menu in ramiho provides a front-end for Midish's MIDI routing, and saving/loading of existing or customized connexions.    
The front-end allows defining connexions not only between ports, but also between channels. You can redirect the MIDI signal coming from one port on a particular channel to any other port, on any MIDI channel. And you can choose to transpose the notes! For example, you could send the output from a MIDI keyboard on channel 1 to a synth that is reading MIDI channel 4 AND to a second synth reading channel 5 a full octave below, so you would have a bass and a sub-bass, like in [this example](https://edpanfleto.com/kdgdkd/git/ramiho_midishrouting.png).     
You can work on the last connexion that you defined or loaded, adding routings, velocity adjustments, send program change signal... And when you are happy, you can save the connexion to be able to access it in the future. Or you can define more complex connexions and filters within the mdshcnx files in ramiho's favcnx directory. All of these files are directly accessible from the Midish menu.  

## Conclusion
ramiho is done with love, but little skill. It is a tool that I did for myself, and has been evolving for years depending on the moment's needs or dreams. It works well, but it probably tries to do too many things, and the code is a bit of a mess. Any comments, suggestions, bug reports and pull requests are welcome.  


## License

GNU General Public License v3.0

# ramiho - Raspberry Pi Midi Host manager

ramiho is a command-line interface designed to simplify MIDI connection management on a headless Raspberry Pi.   
  
For detailed instructions in Spanish, visit [kdg/dkd](http://edpanfleto.com/kdgdkd/).

## Features

ramiho was conceived to ease MIDI connexion management on a headless Raspberry Pi. It is written in bash.   
It builds on Neuma Studio's project [Raspberry Pi as USB/Bluetooth MIDI Host](https://neuma.studio/rpi-midi-complete.html). Neuma's project will automatically connect all MIDI devices on the Raspberry Pi between themselves, which is all you need for simple setups like getting a MIDI keyboard to send notes to a synthesizer.   
My needs are far more complex, so I wrote ramiho as an accessible front-end for aconnect to sort-out MIDI routing issues.   


ramiho's main features are:
- simple, lightweight cli to manage MIDI connexions between devices
- interface for midish connexions, allowing for MIDI routings per channel, merging, splitting... 
- connexion testing tools (MIDI monitors and SendMidi)
- save current connexion configuration, and define alternative favorite setups that can be easily loaded
- upon opening, ramiho will try to load the connexions from the last time it was used   
- all operations are accessible using only the [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png) (numbers and mathematical operators)  
- it talks, it may be alive
  

## Dependencies
ramiho is written in bash, and only requires ALSA for MIDI connexion management. I guess it should work on most Linux distributions, including early Raspberry's. But don't take my word.    
For additional testing functionalities, ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) and [SendMidi](https://github.com/gbevin/ReceiveMIDI), both by Geert Bevin. These tools provide a filtered monitor and the send midi test. The default location for these executables is ramiho's srmidi directory, but you may define any other location (or alias) in the fCONF function.   
For advanced connexion configurations (like routing and and splitting and merging MIDI channels) ramiho uses [midish](https://midish.org), by Alexandre Ratchov.   
And if you wish ramiho to speak back, you will need a text-to-sound engine; I use [festival](http://festvox.org/festival/) (**sudo apt-get install festival**).


## Installation
Copy the files and directories in this repository to your user directory (git clone https://github.com/kdgdkd/ramiho). Make sure that the permissions are right, ramiho is executable and can access favcnx files. You may also install the additional programs mentioned on the previous section.   



## Customization
There are a number of elements that are expected to be customized to the user's own preferences. Open ramiho on a text or code editor.   


**Locale**  
ramiho comes with Spanish and English interfaces. You can switch between them by entering **97** in the main interface. Choose which locale to use by default within the fCONF function (change fLOCALE_EN for fLOCALE_ES to turn the interface to Spanish). 

**Favorite Connexions**  
ramiho allows to load preferred connexion configurations in batch, using the [Favorite Connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnx.png "Favorite Connexions" ) & the midish submenus. You should customize these to have an easy way of loading your own favorite setup configurations.  

ramiho uses four ways of saving connexions: 
- embedded in ramiho's code under three fFAV_x functions, as lists of aconnect commands
- aconsvd files under favcnx directory, as lists of aconnect commands; these files are also used to save current aconnect configurations, so they may get overwritten   
- mdshcnx files under favcnx directory, that allow for advanced routing configuration using midish   
- mdshsvd files under favcnx directory, as midish shell files; there are used to save current midish configurations and may get overwritten   

You may edit these files and functions, adapt the header of the menu (fFAV_HEADER) with a description of the connexions you've set up, and link them with commands (fFAV_ACT). 


## Usage

ramiho generates a dynamic list of available MIDI ports from connected devices, and allows to operate (connect, disconnect, etc) referring only to their position in the list. This results in very lean commands; for example,   
- press **1** to see the list of ports and connexions   
- [**+14**](https://edpanfleto.com/kdgdkd/git/ramiho_connect.png) will send the MIDI signal from port number 1 to port number 4    
- **-\*4** will disconnect every other port from port number 4    

#### using * 
ramiho accepts using **\*** as a symbol for "every other port/device" when connecting & disconnecting. On the terminal   
- **+2\*** will connect port number 2 to every other port   
- **+\*3** will connect every other port to port number 3
- **-3\*** will close connexions from port number 3 to any other port (* is a wildcard)   

### with terminal interface
When ramiho is launched without arguments, it will load ramiho's terminal interface. This is the main interface for ramiho. You will see the Help header with the list of available commands, and the list of MIDI ports and connexions. In English, it should look similar to this:    
<img src="https://edpanfleto.com/kdgdkd/git/ramiho_terminal.png" alt="ramiho_terminal" height="500"/>  

There is already one connexion set, but you'd like to connect device number 2 to device number 4; enter **+24** [like this](https://edpanfleto.com/kdgdkd/git/ramiho_connect2.png "ramiho connect" ).   
If you are happy with the current connexions and want to save them for later use, enter **0** to open the [Favorite Connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnx.png "Favorite Connexions" ) submenu; mine is in Spanish.  
Now enter **03** to [save connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnxsave.png "Save Connexions" ) into file svdcnx3.  
Next time you enter ramiho, you'll just need to send **0** to enter the Favorite Connexions submenu, and then **3** to [load connexions](https://edpanfleto.com/kdgdkd/git/ramiho_favcnxload.png "Load Connexions" ) file svdcnx3.

### with command line
You can access all of ramiho's features through the command line interface, by adding arguments after the ramiho command. Commands mirror the terminal interface's; to show all available commands type
```bash
ramiho 9
```  
and ramiho will print [this](https://edpanfleto.com/kdgdkd/git/ramiho_cli_en.png).   
Arguments will open ramiho's functionalities, just as in the terminal interface; for example, **ramiho +** will open the dialog to create a new connexion.  
You can open and close connexions from the cli with single commands. If you follow **ramiho +** with two numbers, say **ramiho +24**, ramiho will connect port number 2 to port number 4 without opening any dialog. In a similar way, **ramiho -24** would close the previous connexion.   
You can also load your Favorite Connexions with a single command. Enter **ramiho \*n**, with n being a number from 1 to 9, to load presets as defined in fFAV_ACT.


### offline and headless
For some strange reason, ramiho was originally conceived to work from an offline Raspberry Pi, operated with an external numeric pad, providing audio feedback through a small speaker connected to the mini-jack output.  
To work this out, you will need a text-to-sound engine; there's espeak, my preference is for festival. Text will be piped into the tts engine ($tts_on). To configure a different tts engine and to activate sound by default, edit these lines in the script's fCONFIG function:  
```bash
tts_on="festival --tts" 
tts=$tts_on
```  
You can also turn sound on and off by sending **98** in the main interface.   
If you are using an external [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png), you may want to deactivate BloqNum, so the buttons send functions, like arrows, END or PgDn, instead of numbers. Now buttons 1 to 9 represent devices, and if you press button 3 (or rather, PgDn), you'll hear the name of the third device in the list. Operators dealing with connexions work just the same. 

### testing tools
Let's say you connect two MIDI devices with ramiho, but it does not work, the information does not seem to flow, you get no sound. As always, this begs the question "what MIDI channel are you using?"  
ramiho proposes two tools that should help testing input and output MIDI ports on your devices:   
- MIDI monitor - it tests the sending device. It will print MIDI information (like the channel) coming from the sending ports. The first monitor uses ALSA's aseqdump; for the filtered monitors you'll need [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI). In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_monitor.png) ramiho monitors the first port for incoming Control Change signals, and prints the result.      
- Send MIDI - it tests the receiving device. Use ramiho's front-end for [SendMidi](https://github.com/gbevin/ReceiveMIDI) to send notes directly from the Raspberry Pi to your receiving device. In [this example](https://edpanfleto.com/kdgdkd/git/ramiho_sendmidi.png) ramiho sends note 48, on channel 3, to the fourth device in the list (the 'sound' port of a synth).    

If these tests fail, check the cables and whether the hardware is properly configured to send/receive MIDI. Then, you may want to try with Midish.  

### using Midish   
[Midish](https://midish.org/) is a powerful command-line MIDI sequencer/filter by Alexandre Ratchov. It provides advanced MIDI manipulation tools (routing channels, remapping CCs, transposing...), and it also records and plays MIDI. The Midish menu in ramiho provides a front-end for Midish's MIDI routing, and saving/loading of existing or customized connexions.    
The front-end allows defining connexions not only between ports, but also between channels. You can redirect the MIDI signal coming from one port on a particular channel to any other port, on any MIDI channel. And you can choose to transpose the notes! For example, you could send the output from a MIDI keyboard on channel 1 to a synth that is reading MIDI channel 4 AND to a second synth reading channel 5 a full octave below, so you would have a bass and a sub-bass, like in [this example](https://edpanfleto.com/kdgdkd/git/ramiho_midishrouting.png).     
You can work on the last connexion that you defined or loaded, adding routings, velocity adjustments, send program change signal... And when you are happy, you can save the connexion to be able to access it in the future. Or you can define more complex connexions and filters within the mdshcnx files in ramiho's favcnx directory. All of these files are directly accessible from the Midish menu. 

## Conclusion
ramiho solves issues that we, as a band (**PUNKT25**), found when trying to properly configure our DAW-less techno setup. Setting up MIDI connexions between devices, or loading previously used connexion setups, is now easily done with ramiho. My group-members, who wouldn't care about aconnecting anything on a command line, now operate ramiho on ssh clients on their own phones. This brings me a lot of joy. But I think I might be thrilled if I ever learn that this code is helping other people make music, so do not doubt getting in touch if you come to use it. 

## Contributing

Comments, suggestions, bug reports and pull requests are welcome. 


## License

GNU General Public License v3.0

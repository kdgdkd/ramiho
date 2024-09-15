# ramiho - Midi Host manager

ramiho is a simple command-line interface managing ALSA MIDI connexions.     


## Features

ramiho's main features are:
- simple, lightweight cli using ALSA to manage MIDI connexions between devices
- very concise commands, using only the [numeric pad](http://edpanfleto.com/kdgdkd/assets/numpad.png), easing operations from a mobile phone ssh client  
- easily save and load your favorite connexion setups  
- connexion testing tools (MIDI monitors and SendMidi)  
- simple interface for midish, allowing for MIDI routings per channel, merging, splitting... 
- it talks 
  

### Detailed instructions in [ramiho's webpage](https://edpanfleto.com/kdgdkd/ramiho/).

## Installation
ramiho is written in bash, and should work on any Linux distributions using ALSA.   
Clone the repository, navigate to the directory, and make the script executable: 
```bash
git clone https://github.com/kdgdkd/ramiho.git
cd ramiho
chmod +x ramiho
```

[Optional]  
ramiho also works as a simple interface for [midish](https://midish.org), by Alexandre Ratchov.   
For additional testing functionalities, ramiho uses [ReceiveMidi](https://github.com/gbevin/ReceiveMIDI) and [SendMidi](https://github.com/gbevin/ReceiveMIDI)  by Geert Bevin.  
ramiho speaks, but you will need a text-to-sound engine.


## Usage  
You can launch ramiho from the terminal to enter the terminal interface, with a command help header. Like this:

<img src="https://edpanfleto.com/kdgdkd/ramiho/ramiho_terminal.png?" alt="ramiho_terminal" height="564"/>  

You can operate only entering numbers or math operators. For example entering '**+24**' will open the connexion dialog and link the MIDI output of port number 2 to the input in port number 4. The command '**04**' will open the favorite connexions menu, and load a connexion setup you saved as favorite setup number 4.  

You can also use ramiho from the cli, without entering the interface. The same commands apply. So  
**~$ ramiho \***  
will connect every MIDI device detected in the computer, to every other device.


ramiho also provides audio feedback, so it can be operated without a screen, with a speaker.


## 
ramiho is the result more of stubborness than skill. It is a tool that I use extensively, and has been evolving for years depending on the moment's interests or needs. It works, but it probably tries to do too many things, and the code is a bit of a mess. 

Any comments, suggestions, bug reports or contributions are welcome.  


## License

GNU General Public License v3.0

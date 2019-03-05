# fafSTACK   [W.I.P.] (not yet successful)

contributions welcome!

A shell script that installs a so-called fafSTACK on linux
![warty-final-ubuntu](https://user-images.githubusercontent.com/5132359/53690402-25b5a600-3d6a-11e9-94cd-3ac2fc06cf9a.png)

#### What is a fafSTACK?

a faf stack is a set of software and environment settings and folder names and configs allowing you to enjoy the delights of playing [Forged Alliace Forever](https://faforever.com "Forged Alliace Forever") on linux as you would under windows.

such a setup includes (at minimum) :

 - Supreme Commander Forged Alliance
 - (FAF) JAVA client
 - Java 10
 - Gallium-Proton /or/ Proton /or/ Wine (to run the windows game)
 
In the case of this script, we will have recourse to some supplementary items in order to automate our task :

 - curl (fetches installers from git)
 - steamCMD (downloads the game) 
 - steam (installs it)
 
 Dependencies :
 
 - lib32gcc1 (steamcmd)
 - libd3dadapter9-mesa:i386 libd3dadapter9-mesa (Gallium-Proton)

# User-Guide

### 1st

click on installfaf.sh above, then click on "RAW", then type <kbd>Ctrl</kbd> + <kbd>S</kbd>, (you can place anywhere, in home folder is comfortable to work with), then <kbd>Enter</kbd> to save file.

### 2nd

make it executable : (terminal) `chmod +x installfaf.sh`

### 3rd 

run the script within a terminal : `. ./installfaf.sh` (the dot is of important here, without it FAF will fail to run without you personally, running `source ~/.bashrc` and `source /etc/environment` ...minor hassle)

you'll be asked in order :
- your steam account usename
- your steam account password
- your linux session password

### 4rth

at a certain point steam starts up and you'll need to enter Steam Guard.

1. right click on "Supreme Commander - Forged Alliance" in your games Library" -> "SET LAUCNH OPTIONS..." and enter the contents of "the contents of this file are to be pasted in the forged alliance properties launch options" situated in your home folder, just in case, here they are : `PROTON_NO_ESYNC=1, PROTON_DUMP_DEBUG_COMMANDS=1, PROTON_USE_GALLIUM_NINE=1, PROTON_GALLIUM_NINE_MODULEPATH="/usr/lib/i386-linux-gnu/d3d/d3dadapter9.so.1:/usr/lib/x86_64-linux-gnu/d3d/d3dadapter9.so.1" %command%`
2. go into "Steam" -> "Setting" -> "Steam Play" -> "Enable Steam Play for all other titles" (Gallium-nine should already be preselected in th edrop-down.)

it will ask to restart.

when steam starts don't login just close both login windows, this will all the rest of the script to continue.

### 5th

Then it downloads Forged Alliance. this will take a while.
Once it's done steam starts again in order to finalize installation and start (non root hard drive installation is not supported. (E.G.: to a secondary storage HHD, for now I only support installing to SDA.))

accept terms. you can tick the box "Launch game as soon as it's read ready".

Forged Alliance should start, you can set a profile, then set your prefered resulution, then quit the game. (use steam's notification-area icon to close steam once again, to allow the script to continue)

### 6th

FAF should be running.
if not use your trusty terminal :

```
cd ~/faf
./downlords-faf-client
```

then with faf running, log in and set your preferences under "Forged Alliance Forever":
![screenshot from 2019-03-03 04-27-31](https://user-images.githubusercontent.com/5132359/53690540-c0af7f80-3d6c-11e9-9e83-36a25ce7e1f3.png)

"Game Location" :
```
/home/USERNAME/.steam/steam/steamapps/common/Supreme Commander Forged Alliance
```
"Command line format for executable" :
```
/home/USERNAME/.steam/Proton/dist/bin/wine "%s"
```
if this doesn't work try :
```
/home/USERNAME/run "%s"
```


"Execution Dirrectory" :
```
/home/USERNAME/.faforever/bin
```
where "USERNAME" is what you get when you type `echo $USER` into the terminal.

try to join two games and before the second run in your terminal :

```
chmod +x -R ~/.faforever/*
```
### 7th

GL HF!

# Pre-requisites 

- a steam account with Forged Alliance activated on it [(9420)](https://store.steampowered.com/app/9420/Supreme_Commander_Forged_Alliance/ "Supreme Commander Forged Alliace") 
- a linux system (preferably debian-based such as ubuntu,  for further compatibility please contribute)
- no pre-existing java version (preferably)
- preferably an untouched (or absent) /etc/environement 
- have at least a total of 11.7GB of free space on boot drive
- be willing to write to your boot drive
- be willing to have stuff put into both /etc/environement and .bashrc

## Axes of improvements

- prevent runnning right at the top of the script in all crazy sorts of senarios (like attempting to run on windows or other unsupported distro)
- get rid of behaviors that the user may not be comfortable with; case in point and first offender: giving a shell script sudo priveledges in it's entirety. if the user is warned first hand that he needs curl, steamCMD, steam, lib32gcc1, libd3dadapter9-mesa:i386, libd3dadapter9-mesa beforehand or given an apt install command that opens up ready to run in a seperate terminal (that terminal's outcome being on an "if") so that the user sees firsthand what he's allowing sudo for and what sudo priveledges have been constrained to, then the script can come off less bad practice. Also there should be an opt out of steam username and password entering, in which case at the begining, the script should be able to detect if the condition of Forged Alliance being installed (easy) and first-run have been met (bit of a pickle, it's not as easy as having a config file. although, this requires some testing, perhaps FAF requires nothing more than FA phyisical files, reg entries and perferal runtime libraries may be unused).
- make this script more posix-compliant
- detect linux distro and create if-cases for differing systems (E.G. use yogurt if on an arch-based system instead of apt)
- finish Java 10 workarounds, right now this script only supports use cases for a vanilla ubuntu, or where a user has clubsily removed previous fafSTACK. ideally it would be able to handle keeping a developper's JDK as a neighbor + the config allowing it to be found and default yet somehow still have FAF java client run. hopefully FAF client moves to java 11 and the whole ordeal can be forgotten. then it just become a "do you have java? good! is it 11? no? that's fine i'll just put this jre on and only FAF will use it. The whole java 10 thing is just a nightmare because of the fact the release is frowed upon in the linux community and not in the repos. attempting to install oracle or open jre or jdk **10** via traditional means will **allways** result in your system pulling **11** instead. And the issue with the manual install is that it is a pain to get it to be detected. With apt managing, all this would have been so much easier, the `INSTALL4J_JAVA_HOME` in bashrc would have been sufficient, grting you the leisure to not change user' default java. so bottom right now I don't see the worthwhileness of inversting more time into that part of the script since it would be moot with FAF switching to java 11.
- add support for custom install location. this one is a nightmare. and not facillitated by paths 
- figure out a proper fix to the current lack of path-resolving for FAF's setting "Command line format for executable" than the current workaround: Currently I'm forced to copy Proton Gallium nine's folder to a new folder and point to that new folder instead because the new folder's path doesn't contain dots, numbers or spaces which "Command line format for executable" setting entry in FAF apparently cannot handle. I haven't researched this sufficiently but I suspect there's a better way to do this.
- ideas welcome.

## Not working ?

this is likely. this is still Work in Progress. success rate is basically null at this point.
feedback will help me make fix things, though. this script creates a very minimal log file called "fafstack-versionNumber.log" in your home. you can paste it's contents as part of an issue/bug report.

# Un-installing :

this will very much depend on how much of what you already had installed before running this script that this script has in common.

here are a couple safe bets however, run this in your terminal:

```
rm ~/the\ contents\ of\ this*
rm ~/fafstack-*.log
rm ~/run
rm -rf ~/My\ Games
rm -rf ~/faf
rm -rf ~/.faforever
rm -rf ~/.com.faforever.client.FafClientApplication
grep -v 'LD_PRELOAD' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'DEF_CMD' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'export TERM' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'WINEDLLPATH' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'WINEPREFIX' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'WINEDEBUG' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'SteamGameId' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
grep -v 'SteamAppId' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
sudo apt purge -y steamcmd libd3dadapter9-mesa:i386 libd3dadapter9-mesa lib32gcc1
sudo apt -y autoremove
source ~/.bashrc
```
now all that remains is :

1. steam
if steam wasn't on your system before you ran this script and you are fine with seeing it and Supreme Commander Forged Alliance as well as steamCMD and eventual dependencies go then you can run the following :
```
rm -rf ~/.steam
rm -rf ~/Steam
echo steam steam/purge note '' | sudo debconf-set-selections
sudo apt purge -y steam steamcmd libd3dadapter9-mesa:i386 libd3dadapter9-mesa lib32gcc1
sudo apt -y autoremove
```

2. JUST supreme commander, NOT steam (if you ran n°2 ignore this) :
```
rm -rf ~/.steam/steam/SteamApps/common/Supreme\ Commander\ Forged\ Alliance
```

...will do the trick.

3. keep steam but remove my steam extras such as Gallium9Proton and steamCMD and their dependencies (if you ran n°2 ignore this) : 
```
rm -rf ~/.steam/Proton
rm -rf ~/.steam/compatibilitytools.d
sudo apt purge -y steamcmd libd3dadapter9-mesa:i386 libd3dadapter9-mesa lib32gcc1
sudo apt -y autoremove
```

4. Java 10 (you will have to put back your other versions of java yourself)
```
grep -v 'INSTALL4J_JAVA_HOME' ~/.bashrc > ~/.bashrc2; mv ~/.bashrc2 ~/.bashrc
source ~/.bashrc
sudo rm -rf /usr/lib/jvm/jdk-10.0.2/
sudo update-alternatives --remove-all java
sudo update-alternatives --remove-all javac
```

5. now the only change I made with my script remaining is /etc/environement

I don't store your /etc/environment's original state, sorry I really rushed this script together :D

but if you want a vanilla /etc/environment for ubuntu :

```
sudo rm /etc/environment
sudo bash -c 'echo PATH=\"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games\" >> /etc/environment'
source  /etc/environment
```



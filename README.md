# PiMameArcadeMachine

1.	Buy the panel, and cut in this size. ( check the plan on instructables 
http://www.instructables.com/id/2-Player-Bartop-Arcade-Machine-Powered-by-Pi/ ) 
2.	Buy a RaspberryPi
3.	Mount the panels and make the Cabin 
4.	Install the joystick and buttons
5.	Connect the cables to GPIO
6.	Install Adafruit for the controllers
7.	Configuration files control retrogame.c
8.	Test and Play
9.	Solve problems as Sound or Joystick,Buttons. 
10. Paint and enjoy playing!!! 


9.  <b> Solve problems as Sound or Joystick,Buttons. </b>
Problems found it and Solved: 

I have changed the monitor HVDI to VGA, previously I had sound but when I changed the cable, the sound was mute,  with this command you can test it. 

sudo amixer cset numid=3 1 

A simple change in the config.txt as below and clean sound came from the HDMI jack!! I still don't know why I get static with my sound out of the 3.5mm, but at least now I have a workable solution to get clean sound.

hdmi_drive chooses between HDMI and DVI modes

hdmi_drive=1 Normal DVI mode (No sound)
hdmi_drive=2 Normal HDMI mode (Sound will be sent if supported and enabled)


Another solution but I didn’t try it is:

Create a shell script: sudo nano mpgapless.sh 

|Then enter the following into the shell script.|
|#!/bin/bash # mpgapless  case "$@" in    *.m3u) PL="-playlist"    ;; esac rm /tmp/audiofifo mkfifo /tmp/audiofifo aplay -t raw| |-c 2 -f S16_LE -r 48000 /tmp/audiofifo &> \ /tmp/aplayfifo.log & mplayer -noconfig all -nolirc -nojoystick \ -novideo| |-benchmark -vc null -vo null -ao pcm:fast -af resample=48000 -nocache \ -ao pcm:nowaveheader:file=/tmp/audiofifo $PL "$@"| 

Make the file executable:
chmod 555 ./mpgapless.sh 
If you have a directory of music files you can use it like:
./mpgapless.sh *


error compiling: 

I'm getting an error "fatal error: expat.h: No such file or directory" when trying to build retrogame.c
That is because you are also including the gamera.c in your make which requires the expat and ncurses libraries. Simply type:

sudo apt-get install ncurses-dev libexpat1-dev
then run your make again. you should be good from there.


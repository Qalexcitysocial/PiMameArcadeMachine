#How build Arcade Machine in 10 steps.  PiMameArcadeMachine

*This project is based in another projects on Internet so, you can check diferent methods to build your own arcade machine. 

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


--------- Steps --------- 

1.	Buy the panel, and cut in this size. ( check the plan on instructables 
http://www.instructables.com/id/2-Player-Bartop-Arcade-Machine-Powered-by-Pi/ ) 

2. 	Buy a RaspberryPi, You can buy a RaspberryPi on Amazon, http://www.element14.com/community/community/raspberry-pi 
3. 	Mount the panels and make the Cabin, I took this example. http://www.koenigs.dk/mame/eng/drawweecade.htm
4. 	Install the joystick and buttons, I got the Joystick and Buttons on http://www.arcadeworlduk.com/pages/Shipping-And-Returns.html, I bought the cables and connector in a electronic shop or on the web. 
5. 	Connect the cables to GPIO, to connect the cables on GPIO is more easy that you can imagine, first join just one input of Joystick up, down, left and right, in one cable, and do with buttons the same. check on the GPIO map and connect in Ground. the rest of cable in GPIO inputs. ( check on the map) I connect GPIO17, 23, 24, 25, 6. https://learn.adafruit.com/retro-gaming-with-raspberry-pi/buttons

6. 	Install PiMame OS onto your Raspberry Pi and Install Adafruit for the controllers, go to your Rpi on git and clone, piMame repository. 
- sudo apt-get install git
- git clone https://github.com/ssilverm/pimame_installer
- git clone https://github.com/adafruit/Adafruit-Retrogame
- cd pimame_installer
- sudo ./install.sh
- sudo reboot
(Before installing ROMS onto your Raspberry Pi enter this into your terminal and restart the Pi once again)
- sudo chown -R pi:pi emulators/
- sudo chown -R pi:pi roms/
 NOW!!! Go to the IP Address of your Raspberry Pi in a browser and you will be greeted with the PiMAME web page (See above for   how to find out the IP address of your Raspberry Pi)


7. Configuration files control retrogame.c, go to your project into git/Adafruit-Retrogame and with your best text editor
  open the file, retrogame.c and modify the GPIO position that are your cables. (Joystick and buttons) 

8. Test and Play, at this point you should beable to play only need download some roms from Internet. and Good playing. 

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


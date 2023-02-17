# SAMMI-play-any-sound
A SAMMI extension that you can use to play any type of sound file. Normally only ogg files can be played through SAMMI, however this extension allows you to play a whole range of file-types through the browser all controlled via SAMMI extension commands. These commands also give you more flexibility as well as additional events that are not normally possible through SAMMI's default sound player. It does come with its own limitations however.

#Install:

Download the release version of SAMMI play any sound extension and install using SAMMI's built in extension installer.

Create a folder in your SAMMI/bridge directory to house the sound files you want to use. I simply call my directory sounds, you can call it whatever you want though. Put all of your sounds into this folder. You can also put folders of sounds within that folder as well. The program should pick up any sound files that you put in even if they are in sub directories.

After that your ready to start!

#Usage:

Open the bridge.html file if it isn't already open and switch to the play any sound extension tab. Click the choose files button and navigate to the folder you created during the install. Press the upload key when you are at the correct directory. A table should load with all of your sound files as well as any errors that occurred below the table. If you are getting error when attempting to load sound files, see the troubleshooting section below.

Unfortunately this step must be done every single time you start the bridge. I am looking at alternate ways to do this, but so far they are pretty complex to implement for users, requiring you to setup and run a localized server. This is the easiest solution so far, but I'm open to suggestions from anyone else more experienced.

To use the commands within SAMMI simply open a button's commands menu and click the + sign to make a new command. Go to Extension Commands > SAMMI Bridge. All of the extensions that have been added will be here. I've labeled them all with "SOUND - " preceding the command so as to make it a bit easier for folks with many extensions installed. See below for a list of commands and what they do.

#SAMMI Commands

Field Explanations: A ton of the fields repeat themselves, so I'm going to explain the most common ones here.

  path or nickname: You put the path that shows up in the SAMMI bridge here. You can alternatively use a nickname of your choice if you do choose to set it up for a specific sound.
  
  id: So, just like SAMMI you can play the same sound at the same time. This is a unique id that is given to a specific sound playing for that path/nickname. Even though it is unique to the path/nickname it is not unique globally, so you still need the path/nickname to use with the id. You can get the id through extension triggers. See below for more information.

SOUND - nickname
  Description: This command creates a new nickname you can use to refer to a specific sound so that you don't have to remember its path. It's not really necessary for every sound, but if there is a single sound you use a lot of you can use this to make it a bit easier to remember its reference.
  Fields: 
    path: Required
    nickname: Required
    
SOUND - play
  Description: This is pretty self explanatory but here is the explanation anyway. Plays the sound.
  Fields: 
    path or nickname: Required
    
SOUND - stop
  Descripton: Stop a sound that is currently playing. If no id is specified it stops ALL sounds of that specific path/nickname.
  Fields: 
    path or nickname: Required
    id: Optional
    
SOUND - pause
  Description: Pause a sound that is currently playing. If no id is specified it pauses ALL sounds of that specific path/nickname.
  Fields:
    path or nickname: Required
    id: Optional
    
SOUND - resume
  Description: This will resume a sound after it has been paused. One of the few commands that requires an id.
  Fields:
    path or nickname: Required
    id: Required
    
SOUND - mute
  Description: This will mute/unmute a sound while it is currently playing. If no id is given it mutes/unmutes ALL sounds of the specific path/nickname.
  Fields:
    path or nickname: Required
    id: Optional
    mute/unmute: Checked mutes, Unchecked unmutes
    
SOUND - 
    
  

#SAMMI Extension Triggers

#Troubleshooting

This is a work in progress so bear with me. If you are having a problem not listed here send me a message on discord @failstream#2571 and I'll get back to you when I can.

Errors Loading Files:

ErrorCode 4 : If you are getting this error code then the Howler.js library that I used does not like your file for some reason. I'm not sure why that might be as I didn't make it. You could open the file in a program like Audacity and just save a new copy without modifying it. That has worked for me so far.

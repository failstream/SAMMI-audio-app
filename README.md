# SAMMI-play-any-sound
A SAMMI extension that you can use to play any type of sound file. Normally only ogg files can be played through SAMMI, however this extension allows you to play a whole range of file-types through the browser all controlled via SAMMI extension commands. These commands also give you more flexibility as well as additional events that are not normally possible through SAMMI's default sound player.

It does come with its own limitations however. Currently it loads all sounds that are selected into memory and it stays there. This is out of necessity due to permissions. If I didn't do it this way the user would have to click EVERY time a new sound is loaded. I am looking for a way to mitigate this, so anyone who has any suggestions I'm all ears. For now I wouldn't recommend using this with too many sounds. I haven't really pushed the limits of it, but I assume everyone's computer will have a different limit on how many sounds could be loaded at once without affecting performance.

# Install:

Download the release version of SAMMI play any sound extension and install using SAMMI's built in extension installer.

Create a folder in your SAMMI/bridge directory to house the sound files you want to use. I simply call my directory sounds, you can call it whatever you want though. Put all of your sounds into this folder. You can also put folders of sounds within that folder as well. The program should pick up any sound files that you put in even if they are in sub directories.

After that your ready to start!

# Usage:

Open the bridge.html file if it isn't already open and switch to the play any sound extension tab. Click the choose files button and navigate to the folder you created during the install. Press the upload key when you are at the correct directory. A table should load with all of your sound files as well as any errors that occurred below the table. If you are getting error when attempting to load sound files, see the troubleshooting section below.

Unfortunately this step must be done every single time you start the bridge. I am looking at alternate ways to do this, but so far they are pretty complex to implement for users, requiring you to setup and run a localized server. This is the easiest solution so far, but I'm open to suggestions from anyone else more experienced.

To use the commands within SAMMI simply open a button's commands menu and click the + sign to make a new command. Go to Extension Commands > SAMMI Bridge. All of the extensions that have been added will be here. I've labeled them all with "SOUND - " preceding the command so as to make it a bit easier for folks with many extensions installed. See below for a list of commands and what they do.

# SAMMI Commands

Field Explanations: A ton of the fields repeat themselves, so I'm going to explain the most common ones here.

- path or nickname: You put the path that shows up in the SAMMI bridge here. You can alternatively use a nickname of your choice if you do choose to set it up for a specific sound.
  
- id: So, just like SAMMI you can play the same sound at the same time. This is a unique id that is given to a specific sound playing for that path/nickname. Even though it is unique to the path/nickname it is not unique globally, so you still need the path/nickname to use with the id. You can get the id through extension triggers. See below for more information.

Commands:

<br> 

* **SOUND - nickname**
  - Description: This command creates a new nickname you can use to refer to a specific sound so that you don't have to remember its path. It's not really necessary for every sound, but if there is a single sound you use a lot of you can use this to make it a bit easier to remember its reference.
  - Fields: 
  
      `path`      - Required
  
      `nickname`  - Required
    
    <br> 
    
* **SOUND - play**
  - Description: This is pretty self explanatory but here is the explanation anyway. Plays the sound.
  - Fields: 
  
    `path or nickname` - Required
    
    <br> 
    
* **SOUND - stop**
  - Descripton: Stop a sound that is currently playing. If no id is specified it stops ALL sounds of that specific path/nickname.
  - Fields: 
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    <br> 
    
* **SOUND - pause**
  - Description: Pause a sound that is currently playing. If no id is specified it pauses ALL sounds of that specific path/nickname.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    <br> 
    
* **SOUND - resume**
  - Description: This will resume a sound after it has been paused. One of the few commands that requires an id.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Required
    
    <br> 
    
* **SOUND - mute**
  - Description: This will mute/unmute a sound while it is currently playing. If no id is given it mutes/unmutes ALL sounds of the specific path/nickname.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    `mute/unmute`      - Required: Checked mutes, Unchecked unmutes
    
    <br> 
    
* **SOUND - volume**
  - Description: This will set the volume of a sound. If a specific id is passed then it only adjusts the volume for that specific id, otherwise it adjusts for all sounds in that specific path/nickname.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    `volume`           - Required: Slider 0 - 100
    
    <br> 
    
* **SOUND - fade**
  - Description: This will allow you to fade a sound from one volume to another. The duration is in ms. Currently is buggy if you go over 1000ms. I'm looking into it. As with all of these if you omit the id it adjusts for ALL sounds in that specific path/nickname.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    `from`             - Required: Slider 0 - 100 (volume will be set to this prior to starting the fade)
    
    `to`               - Required: Slider 0 - 100 (this is the level the volume will end on)
    
    `duration (ms)`    - Required: How long in milliseconds the fade will take.
 
 <br> 
 
* **SOUND - seek**
  - Description: This will skip to a specific position of a sound. For instance, supposing you had a long song you could skip the intro with this, or send the song back to the start.
  - Fields:
  
    `path or nickname`   - Required
    
    `id`                 - Optional
    
    `position (seconds)` - Required: The position in seconds for where you wish to seek to.
    
    <br> 
    
* **SOUND - rate**
  - Description: This will change the playback rate of the sound. If you omit the id blah blah blah. You get the idea.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    `rate (0.5-4)`     - Required: takes a float from 0.5 to 4. If you choose a variable that isn't valid it gets set to 1 instead.
    
   <br> 
    
* **SOUND - loop**
  - Description: This will set this sound's looping flag to true or false. All sounds are set to false by default. You'll have to change it to true here if you wish a sound to loop. If you don't input an id then it sets all instances of this file/path to looping.
  - Fields:
    `path or nickname` - Required
    `id`               - Optional
    `loop`             - Required: checked loops, unchecked doesn't loop

   <br> 
    
* **SOUND - retrieve info**
  - Description: This will allow you to get information about a specific path/nickname or a specific id. It sends the data to an extensionTrigger of your choice. You'll need another button setup with this trigger to "catch" the data.
  - Fields:
  
    `retrieve`         - Required: The data you wish to retrieve. Right now you can get volume, seek location, rate, state, play status, loop, and duration (in seconds I think) of the sound file. You can also just use the all function to bundle them all up and send them all.
    
    `path or nickname` - Required
    
    `id`               - Optional
    
    `Ext Trigger Name` - Required: If you don't set this then what's the point? Set it to a trigger name and then create another button that gets triggered with it.

    <br>

* **SOUND - add listener**
  - Description: Add a custom event listener that fires for a specific sound and/or a specific id, and a specific event. You can set it to trigger only once or indefinitely. This can let you create buttons that fire off of a sound playing, ending, stopped, when the volume changes, or the play rate changes, or a fade completes.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    `event`            - Required: Dropdown menu with the events that can be selected.
    
    `Ext Trigger Name` - Required: Enter the extension trigger where you wish to receive your info.
    
    `once`             - Required: checked will remove the listener after firing once, unchecked fires indefinitely
    
    <br>
    
* **SOUND - remove listener**
  - Description: Remove the custom event listener that you've already created. It is very important that you enter all the fields exactly the same as when you created the custom listener. For instance, if there is an id in your listener then you must enter the id to remove it. Same if there is no id. You can't leave it blank like other commands and expect all of the events for that path/nickname and event to remove. It must be exact.
  - Fields:
  
    `path or nickname` - Required
    
    `id`               - Optional
    
    `event`            - Required: Dropdown menu with the events that can be selected.
    
    `Ext Trigger Name` - Required: Enter the extension trigger that should be removed.
    
    <br>
    
# SAMMI Default Extension Triggers

OK, so now we get to the really interesting bit. There are tons of events that happen that fires an extension trigger in SAMMI and I'm sure it will allow for some neat effects. Every event is prefixed with "SoundEvent: " in order to allow you to create a button that catches every event from this extension. Keep in mind that the data I pass to these events may still change based on feedback. Some of the data in the objects may be undefined depending on a variety of factors.

* **"SoundEvent: Stop"**  Fires when a sound is stopped. data: `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Mute"**  Fires when a sound is muted/unmuted. data: `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Pause"**  Fires when a sound is paused. data: `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Fade"**  Fires when a fade has completed. data: `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: RateChange"**  Fires when a sound's rate has changed. data: `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: VolumeChange"**  Fires when a sound's volume has changed. data: `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Load"**  Fires when a sound has loaded and is ready to play. This only happens directly after choosing the folder in the browser. This will let you grab the paths in SAMMI without having to be dependant on getting them from the browser or knowing the name of the file. Keep in mind that this happens prior to load errors, so you will want to watch for those even after you get this event. data: `{path}`

* **"SoundEvent: Play"** Fires when a sound has begun playing. `{path, id, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: End"** Fires when a sound has reached the end. Be wary that for looping sounds this will fire every time it reaches the end. `{path, id, looping, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: ERROR -Load"** Fires when there is a load error. This only happens directly after choosing the folder in the browser. You can use this event to filter out files that have loaded with errors and remove them after you've already loaded them with the previous extension. The error number corresponds with a Howler.js error. `{path, id, error}`

* **"SoundEvent: ERROR -Play"** Fires when there is an error trying to play a file. The error number corresponds with a Howler.js error. `{path, id, error}`

# Troubleshooting

This is a work in progress so bear with me. If you are having a problem not listed here send me a message on discord @failstream#2571 and I'll get back to you when I can.

Errors Loading Files:

ErrorCode 4 : If you are getting this error code then the Howler.js library that I used does not like your file for some reason. I'm not sure why that might be as I didn't make it. You could open the file in a program like Audacity and just save a new copy without modifying it. That has worked for me so far.

# SAMMI-audio-app
A SAMMI extension that you can use to play any* type of sound file that your browser supports. Normally only ogg files can be played through SAMMI, however this extension allows you to play a whole range of file-types through the browser all controlled via SAMMI extension commands. These commands also give you more flexibility as well as additional triggers/events that are not normally possible through SAMMI's default sound player.

# Install:

Download the release version of SAMMI play any sound extension and install using SAMMI's built in extension installer.

Create a folder in your SAMMI/bridge directory to house the sound files you want to use. I simply call my directory sounds, you can call it whatever you want though. Put all of your sounds into this folder. You can also go multiple directories deep in the folder, so you can organize your sound files however you want! The only rule is that the sound files must be within the directory or within a child directory of the bridge.html file.

After that you're ready to start!

# Usage:

Open the bridge.html file if it isn't already open and switch to the play any sound extension tab. Click the Load Test button on the extension tab. This must be done each time you start the bridge prior to playing any sounds from SAMMI. It is unavoidable due to the way permissions work in a browser. If you forget to click and then try to play sounds from SAMMI, a popup will remind you the first time it happens.

To use the commands within SAMMI simply open a button's commands menu and click the + sign to make a new command. Go to Extension Commands > SAMMI Bridge. All of the extensions that have been added will be here. I've labeled them all with "SOUND - " preceding the command so as to make it a bit easier for folks with many extensions installed. See below for a list of commands and what they do.

# SAMMI Commands

Field Explanations: A ton of the fields repeat themselves, so I'm going to explain the most common ones here.

- filepath: You put the path that shows up in the SAMMI bridge here. This filepath is relative to your bridge folder. So if you made a folder called "sounds" in your bridge folder and you had file called "test.wav" in there, you would type in "sounds/test.wav" to use it.
  
- id: So, just like in SAMMI you can play the same sound at the same time. This is a unique id that is given to a specific instance of a sound playing for that filepath. Even though it is unique to the filepath it is not unique globally, so you still need the filepath to use with the id. If you enter an id that doesn't exist, nothing should happen at all. You can get the id through extension triggers. See below for more information.

Commands:

<br> 
    
* **SOUND - load sound**
  - Description: This will load the sound to prepare it for playing. This step is not required if you want to simply play a sound. The reason you might want to load the sound first is if you want to make modifications to the sound prior to playing it. Once a sound has been loaded, either through this command or the play command, you can play it again without loading it again until you unload it with the "SOUND - unload sound" command.
  - Fields: 
  
    `filepath` - Required
    
    <br> 

* **SOUND - play**
  - Description: This will play a sound with that filepath. You can also enter an id optionally if you want to resume playing a sound you have paused.
  - Fields: 
  
    `filepath` - Required

    `id`       - Optional
    
    <br> 
    
* **SOUND - stop**
  - Descripton: Stop a sound that is currently playing. If no id is specified it stops all sounds of that specific path
  - Fields: 
  
    `filepath` - Required
    
    `id`       - Optional
    
    <br> 
    
* **SOUND - pause**
  - Description: Pause a sound that is currently playing. If no id is specified it pauses all sounds of that specific path
  - Fields:
  
    `filepath` - Required
    
    `id`       - Optional
    
    <br> 
    
* **SOUND - mute**
  - Description: This will mute/unmute a sound while it is currently playing. If no id is given it mutes/unmutes ALL sounds of the specific path/nickname.
  - Fields:
  
    `filepath`         - Required
    
    `id`               - Optional
    
    `mute/unmute`      - Required: Checked mutes, Unchecked unmutes
    
    <br> 
    
* **SOUND - volume**
  - Description: This will set the volume of a sound. If a specific id is passed then it only adjusts the volume for that specific id, otherwise it adjusts for all sounds in that specific path/nickname.
  - Fields:
  
    `filepath`         - Required
    
    `id`               - Optional
    
    `volume`           - Required: Slider 0 - 100
    
    <br> 
    
* **SOUND - fade**
  - Description: This will allow you to fade a sound from one volume to another. The duration is in ms. Currently is buggy if you go over 1000ms. I'm looking into it, but it might be possible that is the maximum the library allows. As with all of these if you omit the id it adjusts for all sounds in that specific path.
  - Fields:
  
    `filepath`         - Required
    
    `id`               - Optional
    
    `from`             - Required: Slider 0 - 100 (volume will be set to this prior to starting the fade)
    
    `to`               - Required: Slider 0 - 100 (this is the level the volume will end on)
    
    `duration (ms)`    - Required: How long in milliseconds the fade will take.
 
 <br> 
 
* **SOUND - seek**
  - Description: This will skip to a specific position of a sound. For instance, supposing you had a long song you could skip the intro with this, or send the song back to the start. If you omit the id it adjusts for all sounds of the path.
  - Fields:
  
    `filepath`           - Required
    
    `id`                 - Optional
    
    `position (seconds)` - Required: The position in seconds for where you wish to seek to.
    
    <br> 
    
* **SOUND - rate**
  - Description: This will change the playback rate of the sound. If you omit the id it adjusts for all the sounds of that path. Crazy I know.
  - Fields:
  
    `filepath`         - Required
    
    `id`               - Optional
    
    `rate (0.5-4)`     - Required: takes a float from 0.5 to 4. If you choose a variable that isn't valid it gets set to 1 instead.
    
   <br> 
    
* **SOUND - loop**
  - Description: This will set this sound's looping flag to true or false. All sounds are set to false by default. You'll have to change it to true here if you wish a sound to loop. This might surprise you, but if you don't input an id then it sets all instances of this file/path to looping.
  - Fields:

    `filepath`         - Required

    `id`               - Optional

    `loop`             - Required: checked loops, unchecked doesn't loop

   <br> 
    
* **SOUND - retrieve info**
  - Description: This will allow you to get a ton of information about a specific sound. If you enter an id you'll get the info for that specific instance of the sound for that filepath. The object it returns has the following members: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration, trigger}`
  - Fields:
    
    `filepath`         - Required
    
    `id`               - Optional
    
    `Ext Trigger Name` - *Optional: Although this is technically optional either this or the following variable must be set. Use this if you want to use an extension trigger to trigger another button to get the info.

    `varName`          - *Optional: If you fill this in the variable name will be set with the return object.

    `buttonID`         - Optional: Use in conjunction with varName. If this isn't set it will set the return object to a global variable.

    <br>

* **SOUND - add listener**
  - Description: Add a custom event listener that fires for a specific sound and/or a specific id, and a specific event. You can set it to trigger only once or indefinitely. This can let you create buttons that fire if a sound plays, ends, is stopped/paused, when the volume changes, when the play rate changes, a fade completes or the seek header is changed. When it sends the trigger it also sends an object with the following fields: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration, trigger}`
  - Fields:
  
    `filepath`         - Required
    
    `id`               - Optional
    
    `event`            - Required: Dropdown menu with the events that can be selected.
    
    `Ext Trigger Name` - Required: Enter the extension trigger where you wish to receive your info.
    
    `once`             - Required: checked will remove the listener after firing once, unchecked fires indefinitely
    
    <br>
    
* **SOUND - remove listener**
  - Description: Remove the custom event listener that you've already created. It is very important that you enter all the fields exactly the same as when you created the custom listener. For instance, if there is an id in your listener then you must enter the id to remove it. Same if there is no id. You can't leave it blank like other commands and expect all of the events for that path and event to remove. It must be exact.
  - Fields:
  
    `filepath`         - Required
    
    `id`               - Optional
    
    `event`            - Required: Dropdown menu with the events that can be selected.
    
    `Ext Trigger Name` - Required: Enter the extension trigger that should be removed.
    
    <br>

* **SOUND - unload sound**
  - Description: This will unload a sound so that it can no longer be played or accessed. Why would you want to do this? Not sure. I included it as an option anyway.
  - Fields: 
  
    `filepath` - Required
    
    <br> 

# SAMMI Default Extension Triggers

These events fire for every single sound regardless of path/id. Every event is prefixed with "SoundEvent: " in order to allow you to create a button that catches every event from this extension. Some of the data in the objects may be undefined or null under some conditions. If you want to fire an event for only a specific sound/instance of a sound, see the "SOUND - add listener" command above.

* **"SoundEvent: Stop"**  Fires when a sound is stopped. data: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Mute"**  Fires when a sound is muted/unmuted. data: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Pause"**  Fires when a sound is paused. data: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Fade"**  Fires when a fade has completed. data: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: RateChange"**  Fires when a sound's rate has changed. data: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: VolumeChange"**  Fires when a sound's volume has changed. data: `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: Load"**  Fires when a sound has begun loading and is ready to play. Keep in mind that this happens prior to load errors, so you will want to watch for those even after you get this event. data: `{path, event, state}`

* **"SoundEvent: Play"** Fires when a sound has begun playing. `{path, id, event, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: End"** Fires when a sound has reached the end. Be wary that for looping sounds this will fire every time it reaches the end. `{path, id, event, looping, playing, loop, seek, rate, volume, mute, state, duration}`

* **"SoundEvent: LoadError"** Fires when there is a load error. The error number corresponds with a Howler.js error code. If this event fires the sound is automatically unloaded. `{path, id, error}`

* **"SoundEvent: PlayError"** Fires when there is an error trying to play a file. The error number corresponds with a Howler.js error. `{path, id, event, error}`

# Troubleshooting

This is a work in progress so bear with me. If you are having a problem not listed here create a post on the SAMMI discord under the SAMMI helpdesk in the extensions section or send me a message on discord @failstream#2571 and I'll get back to you when I can.

1) I keep getting a SAMMI popup that says "Cannot play sound due to non-interaction with bridge"

* Exactly what it says! Due to the way that HTML pages/permissions work, it can't actually do anything until you give it permission. This isn't something that can be fixed unless you want to run the bridge off of a webserver on your computer. Luckily, all you have to do is click the "Load Test" button on the bridge. You don't actually have to load anything, just click the button. Afterwards the extension will be able to play any sounds you load from SAMMI without your input. If you want to avoid the popup altogether, just click that button before playing any sounds through SAMMI.

2) I'm getting an error code 4 when loading one of my sound files!

* If you are getting this error code then the Howler.js library that I used does not like your file for some reason. I'm not sure why that might be as I didn't make it. You could open the file in a program like Audacity and just save a new copy without modifying it. That has worked for me so far.

# Changelog

* v0.6
  1) Fixed several load/unload bugs and table element bugs. Unloading and loading sounds should work without problems now.
  2) Fixed a case where play triggers were sometimes not happening when triggering from the deck.
  3) Normalized the error trigger names to be more in line with the other trigger names. "ERROR  -Load" is now "LoadError" and "ERROR  -Play" is now "PlayError"
  4) Retrieve Info now has the option to either set a variable or trigger an extension or both in order to retrieve the object.
  5) Created and added the Audio App Testing deck v1.0.

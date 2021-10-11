MOVED to https://git.interhacker.space/protonphoton/miredis

# Forward Midi events to redis and OSC

Miredis hooks to all midi devices connected and listen for events. All events are forwarded to your redis server and your OSC server. 

Miredis can optionnaly hook to opensourced Link protocol (200+ music & videos apps) -> "/beat" & "/bpm".

Custom events can be triggered with reranged values from one or two CC. Szz custom action types



![Clitools](https://www.teamlaser.fr/images/miredispad.png)

Run (not with python 3.9)

python3 miredis.py

See options :

python3 miredis.py -h

To enable Link : 

python3 miredis.py -link

(for cheap midi interface midisport/midiman support on Linux : apt-get install midisport-firmware)

## New Features

- Midi timecode thks to https://github.com/jeffmikels/timecode_tools.git
- CC values can be reranged, see 'custom action type'
- LJ2 custom actions for laser settings (midichannel -> laser number)
- Midi controlled python example : midicontrol.py
- Verbose mode -v
- Redis subscribe events
- Clitools program selection mode for Launchpads
- Cstom redis 'key event'. To be more semantic/hardware agnostic with configuration file  miredis.json. i.e  "/feedback/1/114/value" is generated each time a CC message on channel 1/ CC 114 is received.

## Custom action types (see miredis.json)

Process given CC(s), create and send result with a "name" event. Builtin types with mandatory parameters :

- 0   : note number (name, type, note)
- 7   : rerange 7 bits midi cc (0-127) to a lowend-highend number (name, type, CC, low end, high end, default value)
- 8   : rerange 7 bits midi cc (0-127) to 8 bits number (0-255) (name, type, CC, default value)
- 140 : rerange one midi cc (0-127) to 14 bits number (0-16383) (name, type, CC, default value)
- 14  : 2 cc (14 bits value) reranged to a lowend-highend number (name, type, highCC, lowCC, low end, high end, default value)

## OSC

Following messages will be sent to an outside OSC server :

/midi/noteon midichannel note velocity

/midi/noteoff midichannel note 

/midi/cc midichannel ccnumber value

/midi/clock

/midi/start

/midi/stop

/beat beatnumber

/bpm   bpm

/Your custom (see configuration file). Ex : /feedback midichannel ccnumber value


## Redis keys

"/midi/noteon/midichannel"      value : "note/velocity"

"/midi/noteoff/midichannel"     value : "note"

"/midi/cc/midichannel/ccnumber" value : "ccvalue"

"/beat"							value : "beatnumber"

"/bpm"							value : "currentbpm"

custom ones

## Redis : midi events published to "/midi/last_event"

"/midi/noteon/midichannel/note/velocity"

"/midi/noteoff/midichannel/note

"/midi/cc/midichannel/ccnumber/ccvalue"

customs one like "/feedback/1/114/value" 

## Custom events examples : LJ2 settings for midi controller

Buttons :

- grid/lasernumber           note on 33
- black/lasernumber          note on 35
- swap/X/lasernumber         note on 24
- swap/Y/lasernumber         note on 26

Sliders : 

- kpps/lasernumber           cc 0 cc 1
- angle/lasernumber          cc 2 (0-360)
- scale/X/lasernumber value  cc 4 (0-200)
- scale/Y/lasernumber value  cc 5 (0-200)
- red/lasernumber            cc 8 (0-255)
- green/lasernumber          cc 9 (0-255)
- blue/lasernumber           cc 10 (0-255)
- intensity/lasernumber      cc 11 (0-255)
- loffset/X/lasernumber      cc 12  cc 13  (-27000, 27000)
- loffset/Y/lasernumber      cc 14  cc 15  (-27000, 27000)

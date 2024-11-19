# LGWebOSRemote
Command line webOS remote for LGTVs. This tool uses a connection via websockets to port 3000 on newer LG TVs, there are other tools which use a restful connection to port 8080 however that port is closed on newer firmware versions.

## A note from the developer

My LG TV is now so out of date that largely what is developed here is tested, improved and debugged by the community. As it goes my TV works fine and I'm not the kind of person to create more unnecessary electrical waste than I need to so as long as my current TV works, it's largely down to you guys.

A big thanks for the contributions over the years too, lots of people have made lots of changes to this project over time, and it would only be as useful as it is with their help.


## Supported models

### Tested with

  * 43LM6300PSB
  * 43UN73003LC
  * 43UJ630V-ZA
  * 60UJ6300-UA
  * HU80KG.AEU (CineBeam 4K)
  * OLED48A2
  * OLED55B7
  * OLED55C9
  * OLED55CX5LB
  * OLED55CXAUA
  * OLED65B9PUA
  * OLED77CX9LA
  * OLED77GX
  * OLED48C1 (ssl)
  * OLED42C2 (ssl)
  * OLED48C2 (ssl)
  * SK8500PLA
  * SM9010PLA
  * UF776V
  * UF830V
  * UH650V
  * UU668V
  * UJ6309
  * UJ635V
  * UJ6570
  * UJ701V
  * [please add more!]

Tested with python 3.9 on Debian Unstable.
Tested with python 3.10 on Windows 10/11
Tested with 3.10 on WSL (Ubuntu 20.04)
Tested with python 3.12 on macOS

### Likely supports

All devices with firmware major version 4, product name "webOSTV 2.0"

## Available Commands
	lgtv scan
	lgtv  auth <host> tv
	lgtv setDefault tv
	lgtv --name tv  audioStatus
	lgtv --name tv  audioVolume
	lgtv --name tv  closeAlert <alertId>
	lgtv --name tv  closeApp <appid>
	lgtv --name tv  createAlert <message> <button>
	lgtv --name tv  execute <command>
	lgtv --name tv  getCursorSocket
	lgtv --name tv  getForegroundAppInfo
	lgtv --name tv  getPictureSettings
	lgtv --name tv  getPowerState
	lgtv --name tv  getSoundOutput
	lgtv --name tv  getSystemInfo
	lgtv --name tv  getTVChannel
	lgtv --name tv  input3DOff
	lgtv --name tv  input3DOn
	lgtv --name tv  inputChannelDown
	lgtv --name tv  inputChannelUp
	lgtv --name tv  inputMediaFastForward
	lgtv --name tv  inputMediaPause
	lgtv --name tv  inputMediaPlay
	lgtv --name tv  inputMediaRewind
	lgtv --name tv  inputMediaStop
	lgtv --name tv  listApps
	lgtv --name tv  listLaunchPoints
	lgtv --name tv  listChannels
	lgtv --name tv  listInputs
	lgtv --name tv  listServices
	lgtv --name tv  mute <true|false>
	lgtv --name tv  notification <message>
	lgtv --name tv  notificationWithIcon <message> <url>
	lgtv --name tv  off
	lgtv --name tv  on
	lgtv --name tv  openAppWithPayload <payload>
	lgtv --name tv  openBrowserAt <url>
	lgtv --name tv  openYoutubeId <videoid>
	lgtv --name tv  openYoutubeURL <url>
	lgtv --name tv  openYoutubeLegacyId <videoid>
	lgtv --name tv  openYoutubeLegacyURL <url>
	lgtv --name tv  sendButton <button>
	lgtv --name tv  serialise
	lgtv --name tv  setInput <input_id>
	lgtv --name tv  setSoundOutput <tv_speaker|external_optical|external_arc|external_speaker|lineout|headphone|tv_external_speaker|tv_speaker_headphone|bt_soundbar>
	lgtv --name tv  screenOff
	lgtv --name tv  screenOn
	lgtv --name tv  setTVChannel <channelId>
	lgtv --name tv  setVolume <level>
	lgtv --name tv  startApp <appid>
	lgtv --name tv  swInfo
	lgtv --name tv  volumeDown
	lgtv --name tv  volumeUp

## Install

Requires wakeonlan, websocket for python (python3-websocket for python3), and getmac.
python-pip (python3-pip for python3) and git are required for the installation process.

    python -m venv lgtv-venv
    source lgtv-venv/bin/activate
    pip install git+https://github.com/klattimer/LGWebOSRemote

To install it system wide:

	sudo mkdir -p /opt
	sudo python -m venv /opt/lgtv-venv
	source /opt/lgtv-venv/bin/activate
	sudo pip install git+https://github.com/klattimer/LGWebOSRemote

or with [pipx](https://pipx.pypa.io/stable/):

	pipx install git+https://github.com/klattimer/LGWebOSRemote.git

## Example usage
    # Scan/Authenticate
    $ lgtv scan
    {
        "count": 1,
        "list": [
            {
                "address": "192.168.1.31",
                "model": "UF830V",
                "uuid": "10f34f86-0664-f223-4b8f-d16a772d9baf"
            }
        ],
        "result": "ok"
    }
    $ lgtv  auth 192.168.1.31 tv
    # At this point the TV will request pairing, follow the instructions on screen

    # Commands are basically
    $ lgtv --name TVNAME  COMMAND COMMAND_ARGS

    $ lgtv --name tv  on
    $ lgtv --name tv  off

    # If you have the youtube plugin
    $ lgtv --name tv  openYoutubeURL https://www.youtube.com/watch?v=dQw4w9WgXcQ

    # Otherwise, this works reasonably well
    $ lgtv --name tv  openBrowserAt https://www.youtube.com/tv#/watch?v=dQw4w9WgXcQ

    # You can set the default TV so the `--name` argument can be skipped
    $ lgtv setDefault tv

## SSL

Starting 25th of January 2023 LG has deprecated insecure ws connections, ssl is now required. Because of this, should you wish to use it on newer firmware devices you can append the argument "ssl" at the back. It connects to 3001 with wss. 

### Example
```
$ lgtv auth 192.168.1.31 tv
$ lgtv --name tv  off
$ lgtv --name tv  screenOff
```

sendButton args:
['asterisk', 'back', 'blue', 'channel_down', 'channel_up', 'click', 'down', 'enter', 'exit', 'fast_forward', 'green', 'home', 'left', 'pause', 'play', 'red', 'rewind', 'right', 'stop', 'up', 'volume_down', 'volume_up', 'yellow']


## Caveats

You need to auth with the TV before being able to use the on command as it requires the mac address.

## TODO

Implement the following features:

	closeToast
	getSystemSettings

## Bugs

I couldn't test youtube because it seems the app isn't installed and not available to download right now
maybe they're updating it?

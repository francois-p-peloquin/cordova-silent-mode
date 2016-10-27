## Cordova Silent Mode

This plugin allow to verify, through phonegap, if the current device is in silent mode. Pleas note that this only works for iOS at the moment.

## Installation

```bash
cordova plugin add https://github.com/francois-p-peloquin/cordova-silent-mode
```

## Usage

```js
//Must be run before you wish to start observing changes
SilentMode.init();
SilentMode.isMuted(
	function() { //Callback
		console.log('mute enabled'); 
	}, function() {  //Callback
		console.log('mute disabled'); 
	});
```

## Supported Platforms

- iOS

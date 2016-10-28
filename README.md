## Cordova Silent Mode

This Cordova plugin checks if the phone has been set to silent mode. Pleas note that this only works for iOS at the moment.

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

## Other Use Example - Listener Library - ringer.js

```js
var Ringer = function(data) {
	data = typeof data !== 'object' ? {} : data;
	for (var i in data) {
		this[i] = data[i];
	}
	this.init();
};

Ringer.prototype = {
	t: false,
	dur: 50,
	init: function() {
		var Obj = this;
		SilentMode.init();
		Obj.check_ringer();
	},
	check_ringer: function(call) {
		var Obj = this;
		call = typeof call === 'function' ? call : function() {};
		SilentMode.isMuted(function() {
			Obj.cur = true;
			call();
    	},function() {
    		Obj.cur = false;
			call();
    	});
	},
	listen_change: function(call) {
		var Obj = this;
		call = typeof call === 'function' ? call : function() {};
		var r = Obj.cur;
		Obj.t = setInterval(function() {
			Obj.check_ringer(function() {
				if (r != Obj.cur) {
					call();
					clearInterval(Obj.t);
				}
			});
		},Obj.dur);
	},
	end: function() {
		var Obj = this;
		clearInterval(Obj.t);
	},
};
```
## And then...

```js
$$(document).on('deviceready',function() {
	var Ring = new Ringer(); //Will run the Ringer.init() function
	
	//Do other things
	
	var t = setTimeout(function() {
	    Ring.listen_change(function() {
		console.log('the ringer position has changed');
	    });
	},10); //The delay is necessary if called right after instantiating the object 
});
```

## Supported Platforms

- iOS

# trigger.io-ua-push

Urban Airship module for trigger.io platform - integration with push messaging service provided by Urban Airship.
Ported and modified with permission, from the official phonegap plugin.


### project url
[https://github.com/Ansr-io/trigger.io-ua-push](https://github.com/Ansr-io/trigger.io-ua-push)

### project author
petehobo @ gmail DOT com

### project status
Alpha


### setup

First you will need to setup Urban Airship account.

Read the [getting started guide](http://docs.urbanairship.com/dashboard/getting_started.html), and follow the setup steps for ios and Android:

[http://docs.urbanairship.com/build/ios.html](http://docs.urbanairship.com/build/ios.html)

[http://docs.urbanairship.com/build/android.html](http://docs.urbanairship.com/build/android.html)

You should then have 2 configured services:

Apple Push Notification Service (APNS)
Google Cloud Messaging (GCM)


### module config

The module requires three config files. These files should be stored under the `src/` directory of your app, e.g in `src/fixtures/urbanairship/`.


#### AirshipConfig.plist
[http://docs.urbanairship.com/build/ios.html#create-airshipconfig-plist](http://docs.urbanairship.com/build/ios.html#create-airshipconfig-plist)

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>inProduction</key>
  <false/>
  <key>developmentAppKey</key>
  <string>Your Development App Key</string>
  <key>developmentAppSecret</key>
  <string>Your Development App Secret</string>
  <key>productionAppKey</key>
  <string>Your Production App Key</string>
  <key>productionAppSecret</key>
  <string>Your Production App Secret</string>
</dict>
</plist>
```

#### airshipconfig.properties
[http://docs.urbanairship.com/build/android.html#setting-up-gcm-support-for-your-app](http://docs.urbanairship.com/build/android.html#setting-up-gcm-support-for-your-app)

```
gcmSender = Your Google API Project Number (allows multiple senders separated by commas)
transport = gcm
developmentAppKey = Your Development App Key
developmentAppSecret = Your Development App Secret
productionAppKey = Your Production App Key
productionAppSecret = Your Production App Secret
inProduction = false
```

#### location.properties

```
# These settings correspond to the minimum distance and time passed to
# Android's LocationManager when requesting location updates.
updateIntervalMeters = 500
updateIntervalSeconds = 900

# Flag indicating whether the application is allowed to use location.
# You must still enable location inside the application with
# UALocationManager.enableLocation() once your user has opted in.

locationServiceEnabled = true

# If you would rather not have the library using the GPS while tracking
# location, set this to false and the library will only use the network.
# If false, this setting will override the location manager if the best
# provider is determined to be the GPS.

allowGPSForLocationTracking = true

# These settings correspond to the properties of Android's Criteria
# class and are used when determining a location provider.

accuracy = ACCURACY_COARSE
powerRequirement = POWER_LOW
altitudeRequired = false
bearingRequired = false
speedRequired = false
costAllowed = false
```




# API documentation

All methods without a return value return null or undefined.


## Data objects

The Urban Airship javascript API provides standard instances for some of our data. This allows us to clearly explain
what kind of data we're working with when we pass it around throughout the API.

#### Push
```
Push = {
    message: "Your team just scored!",
    extras: {
        "url": "/game/5555"
    }
}
```

#### QuietTime
```
// Quiet time set to 10PM - 6AM
QuietTime = {
    startHour: 22,
    startMinute: 0,
    endHour: 6,
    endMinute: 0
}
```


## Core functions

`forge.urbanairship.<method>`

All methods without a return value return undefined.


#### enablePush: function (success, error) {}
Enable push notifications on the device. This sends a registration request to the backend service.

#### disablePush: function (success, error) {}
Disable push notifications on the device. The device will no longer receive push notifications.

#### enableLocation: function (success, error) {}
Enable location updates on the device.

#### disableLocation: function (success, error) {}
Disable location updates on the device.

#### enableBackgroundLocation: function (success, error) {}
Enable background location updates on the device.

#### disableBackgroundLocation: function (success, error) {}
Disable background location updates on the device.

#### registerForNotificationTypes: function (Bitmask) {}
Note: iOS Only

On iOS, registration for push requires specifying what combination of badges,
sound and alerts are desired. This function must be explicitly called in order
to begin the registration process. For example:

`forge.urbanairship.registerForNotificationTypes(push.notificationType.sound | push.notificationType.alert)`


Available notification types:

notificationType.sound
notificationType.alert
notificationType.badge



## Status functions

#### isPushEnabled: function (callback) {}
Callback arguments : Boolean : enabled

#### isSoundEnabled: function (callback) {}
Note: Android Only

Callback arguments : Boolean : enabled

#### isVibrateEnabled: function (callback) {}
Note: Android Only

Callback arguments : Boolean : enabled

#### isQuietTimeEnabled: function (callback) {}
Callback arguments : Boolean : enabled

#### isInQuietTime: function (callback) {}
Callback arguments : Boolean : enabled

#### isLocationEnabled: function (callback) {}
Callback arguments : Boolean : enabled

#### isBackgroundLocationEnabled: function (callback) {}
Callback arguments : Boolean : enabled



## Getters

#### getIncoming: function ( success, error) {}
Will bring up any existing notification if launched from one
i.e. is only called when the app is NOT running and is launched from a push notification

#### getPushID: function (success, error) {}
Callback arguments: (Object) {valid: Boolean, pushID: String}

Get the push identifier for the device. The push ID is used to send messages to
the device for testing, and is the canoncial identifer for the device in Urban Airship.

Note: iOS will always have a push identifier, Android will have one after successful registration.

#### getQuietTime: function (success, error) {}
Callback arguments : Object : QuietTime

#### getTags: function (success, error) {}
Callback arguments : Array : array of tags

#### getAlias: function (success, error) {}
Callback arguments : String : alias

Get alias for the device.



## Setters

#### setAlias: function (alias, success, error) {}
@alias : String : alias

Set alias for the device.

#### setTags: function (tags, success, error) {}
@tags: Array : tags

The maximum length of a tag is 128 characters.

#### setSoundEnabled: function (enabled, callback) {}
Note: Android Only, iOS sound settings come in the push

@enabled : Boolean : enabled

Set whether the device makes sound on push.

#### setVibrateEnabled: function (enabled, callback) {}
Note: Android Only

Set whether the device vibrates on push.

#### setQuietTimeEnabled: function (enabled, callback) {}
@enabled : Boolean : enabled

Set whether quiet time is on.

#### setQuietTime: function (QuietTime, success, error) {}
QuietTime : Object

Set the quiet time for the device.

#### setAutobadgeEnabled: function (enabled, callback) {}
Note: iOS only

@enabled : Boolean : enabled

Enable/disable the Urban Airship Autobadge feature.

#### setBadgeNumber: function (number, success, error) {}
Note: iOS only

#### recordCurrentLocation: function (callback) {}
Report the location of the device.


## Events

#### Incoming Push

This is called when a push is received, but ONLY while the app is active.
See also `getIncoming()`.

```
forge.internal.addEventListener("urbanairship.pushReceived", function (data) {
   var txt = 'pushReceived: '+JSON.stringify(d);
   console.log(txt);
});
```

#### Registration

Callback arguments: `Object : { valid: Boolean, pushID: String }`

This event is trigerred when a registration response is recieved from UrbanAirship.

```
var registered = false;
forge.internal.addEventListener("urbanairship.registration", function (d) {
        var txt;
        if (!registered) {
            registered = d;
            txt = 'registration: '+JSON.stringify(registered);
            console.log(txt);
        }
});
```



## License (BSD 2-part)

Copyright (c) 2013, On Device Research Ltd.
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
   list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

The views and conclusions contained in the software and documentation are those
of the authors and should not be interpreted as representing official policies,
either expressed or implied, of the FreeBSD Project.

# ioBroker.dysonAirPurifier

![Logo](admin/dyson_logo.svg)![Logo](admin/dyson_pure_cool.jpg)  

![Number of Installations (latest)](http://iobroker.live/badges/dysonairpurifier-installed.svg)
[![NPM version](https://img.shields.io/npm/v/iobroker.dysonairpurifier.svg)](https://www.npmjs.com/package/iobroker.dysonairpurifier)
![Number of Installations (stable)](http://iobroker.live/badges/dysonairpurifier-stable.svg)
[![Known Vulnerabilities](https://snyk.io/test/github/Grizzelbee/ioBroker.dysonairpurifier/badge.svg)](https://snyk.io/test/github/Grizzelbee/ioBroker.dysonairpurifier)
[![Test and Release](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/actions/workflows/test-and-deploy.yml/badge.svg)](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/actions/workflows/test-and-deploy.yml)
[![NPM](https://nodei.co/npm/iobroker.dysonAirPurifier.svg?downloads=true)](https://nodei.co/npm/iobroker.dysonairpurifier/)

[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](https://github.com/grizzelbee/iobroker.dysonairpurifier/blob/master/LICENSE)
[![Downloads](https://img.shields.io/npm/dm/iobroker.dysonairpurifier.svg)](https://www.npmjs.com/package/iobroker.dysonairpurifier)

## ioBroker Adapter for Dyson Air Purifiers and fans

This adapter connects ioBroker to various Dyson Air Purifiers.

Fan-Icon in Logo created by [Freepik](https://www.flaticon.com/de/autoren/freepik) from [www.flaticon.com](https://www.flaticon.com/de/).

### supported devices

* Dyson Pure Cool Link Tower (TP02, ProductType 475)
* Dyson Pure Cool Tower, 2018 model (TP04, ProductType 438)
* Dyson Pure Cool Tower Formaldehyde, 2018 model (TP07, ProductType 438E)
* Dyson Pure Cool Link Desk (DP01, ProductType 469)
* Dyson Pure Cool Desk, 2018 model (DP04, ProductType 520)
* Dyson Pure Hot+Cool Link (HP02, ProductType 455)
* Dyson Pure Hot+Cool Link New (ProductType 455A)
* Dyson Pure Hot+Cool, 2018 model (HP04, ProductType 527)
* Dyson Pure Hot+Cool (HP07, ProductType 527E)
* Dyson Pure Humidify+Cool (PH01, ProductType 358)
## Features

Connects your Dyson fans, fan heaters, air purifiers, and air humidifiers to ioBroker.

* Reads values from devices and sensors
* Can control devices by giving you the ability to change some values (main power, oscillation, heating, fan speed, ...)
* Reads device list from Dyson servers

## Installation

### sentry.io
This adapter uses sentry.io to collect details on crashes and report it automated to the author. The [ioBroker.sentry](https://github.com/ioBroker/plugin-sentry)
plugin is used for it. Please refer to the [plugin homepage](https://github.com/ioBroker/plugin-sentry) for detailed information
on what the plugin does, which information is collected and how to disable it, if you don't like to support the author with
your information on crashes.

### Prerequisites

* This adapter needs Node.js >= version 10
* At least js-Controller 3.0.0 is required
* At least Admin 4.0.9 is required
* To get this adapter running you'll need a Dyson account.
* Make sure to add your fan to your account. Either via App or online.

### Adapter installation

#### Using npm

Run ```npm install iobroker.dysonairpurifier``` on your ioBroker installation to grab the latest version of this adapter from the npm repository.

#### Alternative: Using GitHub URL

Install through the ioBroker Admin UI by pointing it to the latest stable release on GitHub:
<https://github.com/Grizzelbee/ioBroker.dysonairpurifier/tarball/master/>

You can also install older release versions using these methods (by pointing to a version tag, e.g., ```v0.6.0``` instead of ```master```in the URL), but the most recent one is generally preferred.

### Config-data needed

* Dyson account username
* Dyson account password (this adapter can handle passwords up to 32 characters)
* your fans/air purifiers IP address in your LAN.

*Please note*: Due to early development state and a non conform mDNS implementation by Dyson you'll need to provide the local IP of the device *after the first run*.

*Additional Note*: Since Version 0.7.1 the adapter tries to connect to the device by its hostname (serial number) when no host address/IP ist given. This will work under two prerequisites:
1. There is a DNS Server running in your LAN. Either in your router (e.g. FritzBoxes have a DNS running) or a dedicated one.
2. You haven't changed the default device name.

> On the first start of this adapter the Dyson API is queried for all your devices and all supported devices will be created in the devicetree -- with their basic information provided by the API and an additional field "Hostaddress".
>
> So please run the adapter once, and your Dyson devices will be created in the device tree with their basic settings.
>
> Then stop the adapter, enter the IP(s) into the Hostaddress field(s) and restart the adapter. After that your Dyson devices in the device tree should be populated with data.

### 2-factor Authentication (since V0.9.0)
After installation of the adapter it should be started automatically - if not please start it first. 
After an update it will also restart automatically. In both cases it will remain in "yellow" state
and probably show some errors in the log - that's fine for now.
* Open the config dialog of the adapter
* At least fill in your eMail address, the password and the country code - the rest is optional  
* Click the 2FA-Code Email button to initiate the process
* You'll receive a "challengeId" automatically in the according field, an eMail and a dialog with further instructions
* enter the 6-digit code from the eMail into the field "dyson one time password"
* Click the "Finish" button
* after that you should have received a token from dyson (invisible for security purposes)
* Click save & close after you have completed your setup - the adapter should start anew and turn green.

All the values will be saved and shown furthermore. 
> Usually you don't need to do this 2 FA on a scheduled basis - but you may repeat it when needed.

#### If you are facing the 401 issue during 2-FA. Please try this workaround:
1. Log out of your dyson smartphone app
2. Wait a few minutes
3. Enter your login data to the adapter (if not already done) and follow the 2FA procedure to the end.
4. Adapter should start and turn green.
5. wait a while (up to an hour or maybe more since dyson has a blocker for too many requests in a short time frame)
6. Login back into your dyson smartphone app if you like to use it.
 
## Controlling your device(s)
This adapter is currently able to control the following states of your devices:
* FanMode                   , Mode of device (Manual, Auto, Off)
* FanSpeed                  , Current fan speed
* Nightmode                 , Night mode state
* Oscillation               , Oscillation of fan (On, Off).
* OscillationRight          , OscillationAngle Upper Boundary
* OscillationLeft           , OscillationAngle Lower Boundary
* OscillationAngle          , OscillationAngle
* ContinuousMonitoring      , Continuous Monitoring of environmental sensors even if device is off.
* MainPower                 , Main Power of fan.
* AutomaticMode             , Fan is in automatic mode.
* Flowdirection             , Direction the fan blows to. ON=Front; OFF=Back (aka Jet focus)
* Jetfocus                  , Direction the fan blows to. ON=Front; OFF=Back (aka Jet focus)
* HeatingMode               , Heating Mode [ON/OFF]
* HeatingTargetTemp         , Target temperature for heating
* AirQualityTarget          , Target air quality for auto mode.
* HumidificationMode        , On / Off
* HumidifyAutoMode          , Auto / Off
* AutoHumidificationTarget  , Auto HumidificationTarget
* HumidificationTarget      , Manual HumidificationTarget
* TemperatureUnit           , Unit to display temperature values in (Fan display).
* WaterHardness             , Soft, Medium, Hard

Possible values for these states are documented below, as far as known.
Fan speed only allows values from 1 to 10 and Auto. If you like to set your fan speed down to 0 you'll need to power off the main power.
Which is what the dyson app does also.

### Known issues
* No automatic IP detection of devices

## Changelog

### V2.1.4 (2021-10-20) (Running to the edge)
* (grizzelbee) New: [#152](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/152) Added token-indicator to config page in admin to show whether a token has already been received and saved or not.

### V2.1.3 (2021-10-17) (Running to the edge)
* (grizzelbee) Fix: [#148](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/148) Hostaddress is used properly when given.
* (grizzelbee) Fix: [#149](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/149) OscillationAngle "Breeze" is working now 
* (grizzelbee) Fix: [#150](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/150) Strange delay and jumping of boolean switches is fixed 

### V2.1.2 (2021-10-07) (Running to the edge)
* (grizzelbee) New: Removed NO2 from general AirQuality to be more compliant to dyson-app
* (grizzelbee) Upd: Code cleanup
* (grizzelbee) Upd: Removed delay between sending a command and new values getting displayed (max 30 Secs)


### V2.1.1 (2021-10-05) (Running to the edge)
* (grizzelbee) New: Added some more data points 
* (grizzelbee) New: Added switch for temperature unit of the fan display
* (grizzelbee) New: Improved logging of unknown data points
* (germanBluefox) Fix: Fixed icon links
* (grizzelbee) Fix: fixed dependencies badge
* (grizzelbee) Fix: added missing dependency plugin-sentry
* (grizzelbee) Fix: Setting HumidificationTarget now works

### V2.0.1 (2021-10-04) (Lost in forever)
* (grizzelbee) Fix: Turning on HeatingMode should work now
* (grizzelbee) Fix: Sentry-error [DYSONAIRPURIFIER-7](https://sentry.io/organizations/nocompany-6j/issues/2690134161/?project=5735771) -> Cannot read property '3' of undefined
* (grizzelbee) Upd: Updated dependencies

### V2.0.0 (2021-09-26) (Lost in forever)
* (grizzelbee) New: Added DeepCleanCycle to known data points
* (grizzelbee) Fix: Switching water hardness now really works
* (grizzelbee) BREAKING CHANGES: Please recreate your object tree and test your scripts!
* (grizzelbee) Chg: All ON/OFF switches are now boolean types to be more compliant to ioBroker standards for VIS and other adapters. Please delete those data points and let them being recreated if necessary.
* (grizzelbee) Chg: All angles are numbers now
* (grizzelbee) Chg: All 2-way switches are boolean now
* 
### V1.1.0 (2021-09-15) (Coming home)
* (grizzelbee) New: Added correct tier-level to io-package
* (grizzelbee) New: improved logging of unknown data points
* (grizzelbee) New: Added support for dyson Pure Hot+Cool Link (ProductType 455A) 
* (grizzelbee) New: Added support for formaldehyde sensor
* (grizzelbee) New: oscillation angles can be set
* (grizzelbee) Upd: Improved OscillationAngle data point to display only the values supported by the current model  
* (grizzelbee) Fix: removed info: undefined is not a valid state value for id "Hostaddress"

### V1.0.0 (2021-08-26) (Dim the spotlight)
* (grizzelbee) Fix: [#130](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/130) Fixed the newly introduced bug showing wrong values for temperatures
* (grizzelbee) Upd: Pushed to version 1.0.0
* (grizzelbee) Upd: Updated dependencies

### V0.9.5 (2021-08-23) (Marching on)
* (grizzelbee) Doc: [#124](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/124) Documented workaround for 2FA 401 Issue in ReadMe
* (grizzelbee) Fix: [#128](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/128) Fixed saving of config data
* (grizzelbee) Fix: [#107](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/107) Fixed type error on temperatures
* (grizzelbee) Fix: fixed warnings on startup

### V0.9.4 (2021-08-20) ()
* (grizzelbee) New: [#124](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/124) Credentials won't get logged but shown in a popup in admin when failing 2FA process. 
* (grizzelbee) New: Added adminUI tag to io-package
* (grizzelbee) New: Cleanup of io-package

### V0.9.3 (2021-08-19) (Paralyzed)
* (grizzelbee) New: [#124](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/124) Leading and trailing whitespaces will be removed from all config values when saving
* (grizzelbee) New: [#124](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/124) Password will be logged in clear text in case of a http 401 (unauthorized) error during 2FA
* (grizzelbee) Chg: [#124](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/124) Removed general debug logging of 2FA login data


### V0.9.2 (2021-08-15) (Pearl in a world of dirt)
* (bvol)       New: [#114](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/114) Added Switzerland to country selection in config , Thanks, @BVol, for his code! 
* (grizzelbee) Fix: [#119](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/119) Updated dyson certificate to enable connection again. Thanks, @Krobipd, for helping with the link
* (grizzelbee) Upd: Updated dependencies 

### V0.9.1 (2021-05-17) (Still breathing)
* (grizzelbee) New: [#105](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/105) TP02, HP02 and others supporting the fmod token are now able to switch from Off to Auto- and manual-mode

### V0.9.0 (2021-05-15) (Still breathing)
* (grizzelbee) New: Added ioBroker sentry plugin to report errors automatically 
* (grizzelbee) New: Added support for Dyson Pure Cool TP07 (438E)
* (grizzelbee) New: Added support for Dyson 2-factor login method
* (grizzelbee) New: Added "keep Sensorvalues" to config to prevent destroying old values when continuous monitoring is off and fan is switched off (TP02)  
* (grizzelbee) Fix: FilterLife should now be correctly in hours and percent in two separate data fields for fans supporting this (e.g. TP02)

### V0.8.2 (2021-04-09) (Still breathing)
* (grizzelbee) Fix: [#80](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/80) fixed npm install hint in documentation
* (grizzelbee) Fix: [#82](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/82) fixed common.dataSource type with type >poll<
* (grizzelbee) Fix: [#95](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/95) Added support for dyson Hot+Cool Formaldehyde (527E)
* (grizzelbee) Fix: [#94](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/94) Fixed dustIndex


### V0.8.1 (2021-02-19) (Fall into the flames)
* (grizzelbee) New: added icons to each fan type in device tree
* (grizzelbee) New: Showing Filter type correctly - not as code anymore
* (grizzelbee) Upd: updated dependencies

### V0.8.0 (2021-02-18) (Beyond the mirror)
* (grizzelbee) New: Log as info if account is active on login; else log as warning. 
* (grizzelbee) New: [#21](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/21) Improvement for humidifier support
* (grizzelbee) Fix: [#67](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/67) Adapter sometimes wrote objects instead of values.

### V0.7.5 (2021-02-12) (I won't surrender)
* (grizzelbee) Fix: [#65](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/65) Adapter get online again after changes to dyson cloud API login procedure.
* (grizzelbee) New: Adapter reconnects with new host address when it gets changed manually

### V0.7.4 (2021-02-10) (Human)
* (grizzelbee) Fix: fixed adapter traffic light for info.connection
* (grizzelbee) Fix: Minor fixes

### V0.7.3 (2021-02-10) (When angels fall)
* (theimo1221) Fix: [#59](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/59) added default country
* (theimo1221) New: added function to mask password to dyson-utils.js
* (grizzelbee) New: extended config test and error logging
* (grizzelbee) New: added password to protectedNative in io-package.json
* (grizzelbee) Fix: fixed showing password in config (leftover from testing/fixing)
* (grizzelbee) Fix: fixed detection of needed js-controller features
* (grizzelbee) Fix: fixed detection if IP is given or not
* (grizzelbee) Upd: creating all data points with await 


### V0.7.2 (2021-02-10) (Songs of love and death)
* (grizzelbee) Fix: [#59](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/59) Fixed bug while loading/saving config which led to wrong values displayed for country and temperature unit
* (grizzelbee) Upd: switched "Skipping unknown ..." message from info to debug 

### V0.7.1 (2021-02-06) (Horizons)
* (grizzelbee) New: When no host address is given - adapter tries to connect via default hostname of the device
* (grizzelbee) Fix: [#13](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/13) Filterlifetime is now correctly displayed in hours and percent for devices supporting this
* (grizzelbee) Fix: [#48](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/48) Fixed countrycodes for UK and USA
* (grizzelbee) Fix: [#52](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/52) Fixed VOCIndex
* (grizzelbee) Fix: Removed option to control Fan state since it corresponds to the state of the fan in auto-mode. Controlling it is senseless.
* (grizzelbee) Fix: Fixed await...then antipattern.
* (grizzelbee) Fix: Fixed undefined roles
* (grizzelbee) Fix: Fixed some bad promises and moved code to dysonUtils
* (grizzelbee) Fix: Fixed encrypting password using js-controller 3.0 build-in routine
* (grizzelbee) Upd: Added topic "Controlling your device(s)" to readme
* (grizzelbee) Upd: Removed unnecessary saving of MQTT password
* (grizzelbee) Upd: [#9](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/9) Added some more dyson codes for heaters and humidifiers


### V0.7.0 (2021-01-08) (Afraid of the dark)
* (jpwenzel)   New: Removing crypto from package dependency list (using Node.js provided version)
* (jpwenzel)   New: Introducing unit tests
* (jpwenzel)   New: At least NodeJs 10.0.0 is required
* (grizzelbee) New: [#23](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/23) - Introduced new data field AirQuality which represents the worst value of all present indexes.
* (grizzelbee) New: BREAKING CHANGE! - switched over to the adapter-prototype build-in password encryption. Therefore, you'll need to enter your password again in config.
* (grizzelbee) New: At least js-controller 3.0.0 is required
* (grizzelbee) New: At least admin 4.0.9 is required
* (jpwenzel)   Fix: General overhaul of readme
* (jpwenzel)   Fix: Code refactoring
* (grizzelbee) Fix: fixed some datafield names - please delete the whole device folder and get them newly created.
* (grizzelbee) Fix: [#18](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/18) - Fixed creating the indexes when there is no according sensor
* (grizzelbee) Fix: [#13](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/13) - Displaying Filter life value in hours again
* (grizzelbee) Fix: [#13](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/13) - Creating additional Filter life value in percent
* (grizzelbee) Fix: removed materializeTab from ioPackage
* (grizzelbee) Fix: calling setState now as callback in createOrExtendObject
* (grizzelbee) Fix: Removed non-compliant values for ROLE
* (grizzelbee) Fix: calling setState in callback of set/createObject now
* (grizzelbee) Fix: ensuring to clear all timeouts in onUnload-function

### V0.6.0 (2020-10-29) (Rage before the storm)
* (grizzelbee) New: [#17](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/17) - Added online-indicator for each device
* (grizzelbee) New: [#19](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/19) - Extended Password length from 15 characters to 32
* (grizzelbee) New: [#20](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/20) - Improved error handling on http communication with Dyson API
* (grizzelbee) Fix: Fixed typo within data field anchorpoint - please delete the old ancorpoint manually.
* (grizzelbee) Fix: [#13](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/13) - Filter life value is now displayed in percent not in hours

### V0.5.1 (2020-10-27) (Heart of the hurricane)
* (grizzelbee) Fix: Added missing clearTimeout

### V0.5.0 (2020-10-27) (Heart of the hurricane)
* (grizzelbee) New: Editable data fields have now appropiate value lists
* (grizzelbee) New: Added more country codes
* (grizzelbee) New: Target temperature of heater can now be set - **in the configured unit!**
* (grizzelbee) Fix: [#13](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/13) - Filter life value is now displayed in percent not in hours
* (grizzelbee) Fix: [#6](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/6) - Changing the fanspeed does now fully work.  

### V0.4.1 (2020-10-16) (unbroken)
* (grizzelbee) New: [#8](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/8) - Documented ProductTypes for better overview and user experience in ReadMe
* (grizzelbee) New: [#9](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/9) - Added some Hot&Cool specific datafields
* (grizzelbee) New: Logging of from devices, when shutting down the adapter
* (grizzelbee) New: [#10](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/10) - Polling device data every X (configurable) seconds for new data, hence sensors don't send updates on changing values
* (grizzelbee) New: [#11](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/11) - Added Austria and France to Country-List
* (grizzelbee) Fix: Fixed bug in error handling when login to Dyson API fails
* (grizzelbee) Fix: [#12](https://github.com/Grizzelbee/ioBroker.dysonairpurifier/issues/12) - Fixed Dyson API login by completely securing via HTTPS.
* (grizzelbee) Fix: Updated some descriptions in config
  
### V0.4.0 (2020-09-29)
* (grizzelbee) New: devices are now **controllable**
* (grizzelbee) New: state-change-messages are processed correctly now
* (grizzelbee) Fix: Added missing °-Sign to temperature unit
* (grizzelbee) Fix: Terminating adapter when starting with missing Dyson credentials
* (grizzelbee) Fix: NO2 and VOC Indices should work now
* (grizzelbee) Fix: Fixed build errors

### V0.3.0 (2020-09-27) - first version worth giving it a try
* (grizzelbee) New: Messages received via Web-API and MQTT getting processed
* (grizzelbee) New: datapoints getting created and populated
* (grizzelbee) New: Added config item for desired temperature unit (Kelvin, Fahrenheit, Celsius)
* (grizzelbee) New: Added missing product names to product numbers
* (grizzelbee) New: Hostaddress/IP is editable / configurable
* (grizzelbee) New: calculate quality indexes for PM2.5, PM10, VOC and NO2 according to Dyson App

### V0.2.0 (2020-09-22) - not working! Do not install/use
* (grizzelbee) New: Login to Dyson API works
* (grizzelbee) New: Login to Dyson AirPurifier (2018 Dyson Pure Cool Tower [TP04]) works
* (grizzelbee) New: mqtt-Login to [TP04] works
* (grizzelbee) New: mqtt-request from [TP04] works
* (grizzelbee) New: mqtt-request to [TP04] is responding

### V0.1.0 (2020-09-04) - not working! Do not install/use
* (grizzelbee) first development body (non-functional)

## Explanation of Dyson API data (message payload)

Information copied and extended from <https://github.com/shadowwa/Dyson-MQTT2RRD/blob/master/README.md>

### CURRENT-STATE

| name | meaning | possible values | Unit |
| ------------- | ----- | ----- | ----- |
| mode-reason | Current Mode has been set by RemoteControl, App, Scheduler | PRC, LAPP, LSCH, PUI | |
| state-reason | | MODE | |  
| rssi | WIFI Strength| -100 - 0| dBm|
| channel| WIFI Channel| 52 | |
| fqhp | | 96704 | |
| fghp | | 70480 | |

#### product-state

| name | meaning | possible values | Unit |
| ------------- | ----- | ----- | ----- |
| ercd | Last Error Code | NONE , or some hexa values |  |
| filf | remaining Filter life | 0000 - 4300 | hours|
| fmod | Mode | FAN , AUTO, OFF | |
| fpwr | Main Power | ON, OFF | |
| fnst | Fan Status | ON , OFF, FAN | |
| fnsp | Fan speed | 0001 - 0010, AUTO | |
| fdir | Fandirection aka. Jet focus/ ON=Front, OFF=Back | ON, OFF | |
| ffoc | JetFocus | ON, OFF |
| nmod | Night mode | ON , OFF | |
| oson | Oscillation | ON , OFF| |
| osal | OscillationAngle Lower Boundary | 0005 - 355| °  (degrees)|
| osau | OscillationAngle Upper Boundary | 0005 - 355 | °  (degrees)|
| oscs | OscillationActive | ON, OFF, IDLE | |
| ancp | OscillationAngle  | CUST, 0180 |° (degrees)|
| qtar | Air Quality target | 0001=Good, 0002=Normal, 0003=Bad, 0004=Very bad | |
| rhtm | Continuous Monitoring | ON, OFF | |
| auto | AutomaticMode | ON, OFF | |
| nmdv | NightMode Max Fanspeed? | 0004 | |
| cflr | Status Carbonfilter  | 0000 - 0100 | Percent |
| cflt | Carbonfilter | CARF, NONE | |
| hflr | Status HEPA-Filter | 0000 - 0100 | Percent |
| hflt | HEPA-Filter | GHEP, GCOM | |
| sltm | Sleeptimer | ON, OFF ||
| hmod | Heater Mode [ON/OFF] | HEAT | |
| hmax | Target temperature for heating | 0 .. 5000 | K |
| hume | HumidificationMode     | ON, OFF, |
| haut | Humidify Auto Mode|         HUMIDIFY_AUTO_MODE_ON, HUMIDIFY_AUTO_MODE_OFF |
| humt | Humidification Target| HUMIDIFICATION_MODE_OFF, HUMIDIFICATION_MODE_THIRTY, HUMIDIFICATION_MODE_FORTY, HUMIDIFICATION_MODE_FIFTY, HUMIDIFICATION_MODE_SIXTY, HUMIDIFICATION_MODE_SEVENTY |
| cdrr | CleanDurationRemaining| integer |  minutes |
| rect | AutoHumidificationTarget| integer | % |
| cltr | TimeRemainingToNextClean| integer| hours |
| wath | WaterHardness| SOFT="2025", MEDIUM="1350", HARD="0675"|
| wacd | WarningCode  | NONE... | 
| rstf | reset filter lifecycle | 'RSTF', 'STET',  RESET_FILTER_LIFE_IGNORE, RESET_FILTER_LIFE_ACTION
| corf | Temperature format | ON=Celsius, OFF=Fahrenheit |
| clcr | DeepcleanCycle | CLNO=inactive, CLAC=Deep clean in progress, CLCM=Finished |
| hsta | Heating state | ACTIVE/IDLE |
| msta | Humidification state | Active/Idle   OFF, HUMD |
| psta | [HP0x] Unknown | INIT, CLNG, INV, OFF |
| bril | unknown | 0002 | LEVEL_LOW, LEVEL_MEDIUM, LEVEL_HIGH |    
| fqhp | unknown| |
| tilt | [HP0x] Unknown | string |
| dial | [DP0x] Unknown |  | 


|Error-Codes| Meaning |
| ----- | ----- |
| NONE | There is no error active |
|57C2| unknown |
|11E1| Oscillation has been disabled. Please press Button "Oscillation" on your remote to continue.|

#### scheduler

| name | meaning | possible values | Unit |
| ------------- | ----- | ----- | ----- |
| dstv | daylightSavingTime | 0001... | |
| srsc | ? | 7c68... | |
| tzid | timezone? | 0001... | |

### ENVIRONMENTAL-CURRENT-SENSOR-DATA

#### data

| name | meaning | possible values | Unit |
| ------------- | ----- | ----- | ----- |
| hact | Humidity (%) | 0000 - 0100 | Percent |
| pact | Dust | 0000 - 0009 | |
| sltm | Sleep timer | OFF... 9999 | Minutes |
| tact | Temperature in Kelvin | 0000 - 5000 | K|
| vact | volatile organic compounds | 0001 - 0009 | |
| hcho | Formaldehyde||
| pm25 |  PM2.5 |0018||
| pm10 |  PM10 |0011||
| va10 |  volatile organic compounds|0004||
| noxl |  NO2 |0000 - 0014||
| p25r |  |0019||
| p10r |  |0018||

### ENVIRONMENTAL-AND-USAGE-DATA

Redundant values?

#### data

| name | meaning | possible values | Unit |
| ------------- | ----- | ----- | ----- |
| pal0 - pal9 | number of second spend in this level of dust since the beginning of hour | 0000 - 3600 | |
| palm | seems to be a median value of palX |  | |
| vol0 - vol9 | number of second spend in this level of voc since the beginning of hour | 0000 - 3600 | |
| volm | seems to be a median value of volX |  | |
| aql0 - aql9 | number of second spend in this level of air quality | max (pal, vol)) since the beginning of hour | 0000 - 3600 | |
| aqlm | seems to be a median value of aqlX |  | |
| fafs | seems to be a number of seconds spend in a specific time | 0000 - 3600 | |
| faos | seems to be a number of seconds spend in a specific time | 0000 - 3600 | |
| fofs | seems to be a number of seconds spend in a specific time | 0000 - 3600 | |
| fons | seems to be a number of seconds spend in a specific time | 0000 - 3600 | |
| humm | humidity ? (%) | 0000 - 0100 | |
| tmpm | temperature in kelvin ? | 0000 - 5000 | |

## Legal Notices

Dyson, pure cool, pure hot & cool, and others are trademarks or registered trademarks of [Dyson Ltd.](https://www.dyson.com)
All other trademarks are the property of their respective owners.

## License

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

Copyright (c) 2021 Hanjo Hingsen <open-source@hingsen.de>

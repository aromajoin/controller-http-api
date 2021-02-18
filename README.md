[English](https://github.com/aromajoin/controller-http-api) / [日本語](README-JP.md)

# Aroma Shooter HTTP APIs
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/aromajoin-software/)

Aromajoin's HTTP APIs are available for Aroma Shooters with serial numbers beginning with “ASN2”. Using this API requires a Wi-Fi connection.


## I. Wi-Fi connection setup

There are two methods of connecting an Aroma Shooter to a Wi-Fi network: via Aromajoin's official iOS application or via a web browser.


### Option 1 - Aroma Shooter iOS app

The Aroma Shooter app is available on the Apple App Store:

https://apps.apple.com/app/aroma-shooter/id1477144583

After you install the app, tap the settings button in the bottom left corner of the screen, tap "Others," then tap "Setup AromaShooter's Wi-Fi," and follow the on-screen prompts. On the screen titled "Aroma Shooter Wi-Fi Connection," choose your preferred local Wi-Fi network and enter your credentials. After you receive a message confirming a successful connection, tap the name of the Wi-Fi network again and take note of your device's IP Address -- you'll need this to send requests in Part II. Tap "OK" then "Done" to return to the settings menu.


### Option 2 - Web Browser

Here's an alternative method for people without iOS devices.

After plugging in your Aroma Shooter to a power source, choose it from the available Wi-Fi networks on your computer or mobile device. It will be identifiable by its serial number (For example: "ASN2A5216").

- Password: aromajoin@1003

Using a web browser on your device, navigate to this address: http://192.168.1.1/

From the list of Wi-Fi networks, choose your preferred local network and enter your password. After about 30 seconds, the Aroma Shooter will connect to your local network. Please wait for the success message before refreshing or navigating away from the page. After you receive a message confirming a successful connection, tap the name of the Wi-Fi network again and take note of your device's IP Address -- you'll need this to send requests in Part II. You may now reconnect your computer/phone to the local network. It's time to try sending requests.


## II. APIs

Now your Aroma Shooter is connected to a network through which it may receive HTTP requests, as long as you send the requests from a device on the same network. Using your preferred REST tools, submit requests via the following formats.

**Hostname:** http://[Aroma Shooter IP address] or http://[Device serial].local

**Port:** 1003

The hostname structure should match one of these formats. Please do not copy these examples, as you must modify the IP addresses and/or serial numbers according to your Aroma Shooter(s):

- IP address: http://192.168.1.10:1003 (This format is **recommended**, since it handles requests very quickly.)

- Device serial: http://ASN2A00001.local:1003 (This format may seem intuitive, but it handles requests slowly and is incompatible with Android devices.)


### Example requests:


1. **Get firmware information**

*Path:* /as2/firmware

*Method:* GET

*Header:* “Content-Type: application/json”

*Response sample:*
```javascript
{
  "current": "1.0.0",
  "latest": "1.0.1",
  "internet": "true"

}
```  
  

2. **Diffuse**

*Path:* /as2/diffuse

*Method:* POST

*Header:* “Content-Type: application/json”

*Request body:*

  
```javascript
// For the latest firmware version
{
  "channels": [number, ...], // The cartridge number. Range: 1 ~ 6
  "intensities": [number, ...], // The cartridge intensity as a percentage. Range: 0 ~ 100
  "durations": [number, ...], // Diffusion time in milliseconds. Range: 0 ~ 10000
  "booster": boolean // Set to true to activate the Aroma Shooter's booster fan. Default value is false.
}

// For the firmware lower than v2.0.0 
{
  "duration": int, // Diffusion time in milliseconds. Range: 0 ~ 10000
  "channel": int, // The cartridge number. Range: 1 ~ 6
  "intensity": int, // The cartridge intensity as a percentage. Range: 0 ~ 100
  "booster": boolean // Set to true to activate the Aroma Shooter's booster fan. Default value is false.
}
```

For example:

  
```javascript
{
    "channels": [1,3,5],
    "intensities": [100,50,25],
    "durations": [1000,2000,3000],
    "booster": true
}

// For the firmware lower than v2.0.0
{
  "duration": 3000, // The Aroma Shooter will diffuse for 3 seconds.
  "channel": 3, // The Aroma Shooter will diffuse the contents of the cartridge in slot 3.
  "intensity": 100, // Diffusing at maximum intensity.
  "booster": true // The booster fan will activate.
}
```
  

*Response sample:*

  
```javascript
{
  "status": "done"
}

  ```
  

3. **Stop diffusing**

*Path:* /as2/stop_all

*Method:* POST

*Header:* “Content-Type: application/json”

*Request body:* None

*Response sample:*

  
```javascript
{
  "status": "done"
}
  ```
  
 
----------
Copyright 2020 Aromajoin Corporation under [CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/) license.

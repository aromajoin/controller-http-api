[English](https://github.com/aromajoin/controller-http-api) / [日本語](README-JP.md)

# Aroma Shooter HTTP APIs

Our HTTP APIs are available for Aroma Shooters with serial numbers beginning with “ASN2”. Using this API requires a Wi-Fi connection.


## I. Wi-Fi connection setup

There are two methods of connecting an Aroma Shooter to a Wi-Fi network: via Aromajoin's official iOS application or via a web browser.


### Option 1 - Aroma Shooter iOS app

The Aroma Shooter app is available on the Apple App Store:

https://apps.apple.com/app/aroma-shooter/id1477144583

After you install the app, please follow the steps in this video: https://www.dropbox.com/s/k0ryn34caxqhifh/SetupWiFi_For_AromaShooter.mp4?dl=0.

_NOTE: Your Aroma Shooter cannot maintain simultaneous Bluetooth BLE and Wi-Fi connections. If you are connected to your Aroma Shooter via Bluetooth BLE, disconnect it, check for the ‘No connection’ message at the bottom of the app's main screen, and reset its power by unplugging the power cable. Then, plug the cable back in._

Use the following credentials to establish a connection to your Aroma Shooter.

- SSID: ASN2XXXXX (This is your Aroma Shooter’s serial number. For example: “ASN2A5216”)

- Password: aromajoin@1003


### Option 2 - Web Browser

Here's an alternative method for people without iOS devices.

After plugging in your Aroma Shooter to a power source, choose it from the available Wi-Fi networks on your computer or mobile device. It will be identifiable by its serial number (For example: "ASN2A5216").

- Password: aromajoin@1003

Using a web browser on your device, navigate to this address: http://192.168.1.1/index.html

From the list of Wi-Fi networks, choose your preferred local network and enter your password. After about 30 seconds, the Aroma Shooter will connect to your local network. Please wait for the success message before refreshing or navigating away from the page. You may now reconnect your device to the local network. It's time to try sending requests.


## II. APIs

Now your Aroma Shooter is connected to a network through which it may receive HTTP requests, as long as you send the requests from a device on the same network.

**Hostname:** [Aroma Shooter IP address] or [Device serial].local

**Port:** 1003

For example, the hostname should match the following formats. Please do not copy these examples, as you must modify the IP addresses and/or serial numbers according to your device:

- http://192.168.1.10:1003 (This format is **recommended**, since it handles requests very quickly.)

- http://ASN2A00001.local:1003 (This format may seem intuitive, but it handles requests slowly and is incompatible with Android devices.)


### Example requests:


1. **Get firmware information**

*Path:* [device hostname]:1003/as2/firmware

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

*Path:* [device hostname]:1003/as2/diffuse

*Method:* POST

*Header:* “Content-Type: application/json”

*Request body:*

  
```javascript
{

"duration": Int, // Diffusion time in milliseconds. Range: 0 ~ 10000

"channel": Int, // The cartridge number. Range: 1 ~ 6

"intensity": Int, // The cartridge intensity as a percentage. Range: 0 ~ 100

"booster": Boolean // Set to true to activate the booster, . Default value is false.

}
```

For example:

  
```javascript
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

*Path:* [device hostname]:1003/as2/stop_all

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
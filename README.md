<p align="center">
    <img src="https://i.imgur.com/JBi2C86.png" height="200">
</p>


# Yi!Camera v1

[![npm](https://img.shields.io/npm/v/homebridge-yi-camera.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-yi-camera)
[![npm](https://img.shields.io/npm/dt/homebridge-yi-camera.svg?style=flat-square)](https://www.npmjs.com/package/homebridge-yi-camera)
[![GitHub last commit](https://img.shields.io/github/last-commit/SeydX/homebridge-yi-camera.svg?style=flat-square)](https://github.com/SeydX/homebridge-yi-camera)
[![Donate](https://img.shields.io/badge/Donate-PayPal-blue.svg?style=flat-square&maxAge=2592000)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=NP4T3KASWQLD8)

**Creating and maintaining Homebridge plugins consume a lot of time and effort, if you would like to share your appreciation, feel free to "Star" or donate.**

This is a dynamic platform plugin for [Homebridge](https://github.com/nfarina/homebridge) to control your YI Cameras with MQTT (motion), Telegram notification and FakeGato Support

This Plugin creates one accessory with two services. A Camera service to access your camera via RTSP and a Motion Sensor service with FakeGato functionality to check the last movement.

You can also set up the notifier to get a Telegram notification with customized messages and markdown capability when motion detected/undetected.

## Installation instructions

After [Homebridge](https://github.com/nfarina/homebridge) has been installed:

-  ```(sudo) npm i -g homebridge-yi-camera@latest```


## Basic configuration

 ```
{
  "bridge": {
    ...
  },
  "accessories": [
    ...
  ],
  "platforms": [
    {
      "platform": "YiCamera",
      "debug": false,
      "videoProcessor": "ffmpeg",
      "cameras": [
        {
          "name": "Flur",
          "active": true,
          "videoConfig": {
            "source": "-rtsp_transport tcp -re -i rtsp://192.168.178.31/ch0_0.h264",
            "maxStreams": 2,
            "maxWidth": 1920,
            "maxHeight": 1080,
            "maxFPS": 30,
            "mqtt": {
	          "host":"192.168.178.98",
	          "port":1883,
	          "username":"",
	          "password":"",
	          "recordOnMovement": true,
	          "recordVideoSize": 30
            }
          }
        }
      ]
    }
  ]
}
 ```
 See [Example Config](https://github.com/SeydX/homebridge-yi-camera/blob/master/example-config.json) for more details.

 
 ## Main Config

| **Attributes** | **Required** | **Usage** |
|------------|----------|-------|
| platform | **X** | Must be **YiCamera** |
| debug | | Provides some additional information in the log |
| videoProcessor | | Is the video processor used to manage videos. eg: ffmpeg (by default) or avconv or /a/path/to/another/ffmpeg. Need to use the same parameters than ffmpeg |


 ## Camera Config

| **Attributes** | **Required** | **Usage** |
|------------|----------|-------|
| name | **X** | Unique Camera Name |
| active |  | Activates the camera and exposes to HomeKit (default: false) |
| source | **X** | Source of the stream |
| stillImageSource |  | Source of the latest image (default: source) |
| maxStreams |  | Is the maximum number of streams that will be generated for this camera, default 2 |
| maxWidth |  | Is the maximum width reported to HomeKit, default 1280 |
| maxHeight |  | Is the maximum height reported to HomeKit, default 720 |
| maxFPS |  | Is the maximum frame rate of the stream, default 10 |
| maxBitrate |  | Is the maximum bit rate of the stream in kbit/s, default 300 |
| vcodec |  | If you're running on a RPi with the omx version of ffmpeg installed, you can change to the hardware accelerated video codec with this option, default libx264 |
| audio |  | can be set to true to enable audio streaming from camera. To use audio ffmpeg must be compiled with --enable-libfdk-aac, default false |
| packetSize |  | If audio or video is choppy try a smaller value, set to a multiple of 188, default 1316 |
| vflip |  | Flips the stream vertically, default false |
| hflip |  | Flips the stream horizontally, default false |
| mapvideo |  | Select the stream used for video, default 0:0 |
| mapaudio |  | Select the stream used for audio, default 0:1 |
| videoFilter |  | Allows a custom video filter to be passed to FFmpeg via -vf, defaults to scale=1280:720 |
| additionalCommandline |  | Allows additional of extra command line options to FFmpeg, for example '-loglevel verbose' |

## MQTT Config

| **Attributes** | **Required** | **Usage** |
|------------|----------|-------|
| host | **X** | Address of your MQTT Service |
| port |  | Port of your MQTT Service (default: 1883) |
| username |  | Username for the MQTT Service (If no username setted up, just leave blank) |
| password |  |  Password for the MQTT Service (If no password setted up, just leave blank) |
| recordOnMovement |  | Capture video if movement detected and store to eg /var/homebridge/out.mp4 (default: out.jpg) |
| recordVideoSize |  | Video size in seconds for 'recordOnMovement' |

## Notifier Config

| **Attributes** | **Required** | **Usage** |
|------------|----------|-------|
| active | | Activates/Deactivates notifier
| token | **X** | Telegram Bot Token |
| chatID | **X** | Telegram Chat ID |
| motion_start |  | Own message when motion sensor triggers on (if you dont want to get this notification, just remove from config) |
| motion_stop |  | Own message when motion sensor triggers off (if you dont want to get this notification, just remove from config) |


## Supported clients

This plugin has been verified to work with the following apps on iOS 12.2 and iOS 12.3 Beta:

* iOS 12
* Apple Home
* All 3rd party apps like Elgato Eve etc.
* Homebridge v0.4.49


## Contributing

This plugin uses a modified version of the [homebridge-camera-ffmpeg](https://github.com/KhaosT/homebridge-camera-ffmpeg) plugin from [@KhaosT](https://github.com/KhaosT)

You can contribute to this homebridge plugin in following ways:

- [Report issues](https://github.com/SeydX/homebridge-yi-camera/issues) and help verify fixes as they are checked in.
- Review the [source code changes](https://github.com/SeydX/homebridge-yi-camera/pulls).
- Contribute bug fixes.
- Contribute changes to extend the capabilities

Pull requests are accepted.


## Troubleshooting

If you have any issues with the plugin then you can run this plugin in debug mode, which will provide some additional information. This might be useful for debugging issues. Just open your config.json and set debug to true!
# Lua client for Blynk

[![GitHub version](https://img.shields.io/github/release/vshymanskyy/blynk-library-lua.svg)](https://github.com/vshymanskyy/blynk-library-lua/releases/latest)
[![GitHub download](https://img.shields.io/github/downloads/vshymanskyy/blynk-library-lua/total.svg)](https://github.com/vshymanskyy/blynk-library-lua/releases/latest)
[![GitHub stars](https://img.shields.io/github/stars/vshymanskyy/blynk-library-lua.svg)](https://github.com/vshymanskyy/blynk-library-lua/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/vshymanskyy/blynk-library-lua.svg)](https://github.com/vshymanskyy/blynk-library-lua/issues)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/vshymanskyy/blynk-library-lua/blob/master/LICENSE)

If you like **Blynk** - give it a star, or fork it and contribute! 
[![GitHub stars](https://img.shields.io/github/stars/blynkkk/blynk-library.svg?style=social&label=Star)](https://github.com/blynkkk/blynk-library/stargazers) 
[![GitHub forks](https://img.shields.io/github/forks/blynkkk/blynk-library.svg?style=social&label=Fork)](https://github.com/blynkkk/blynk-library/network)
__________

## What is Blynk?
Blynk provides **iOS** and **Android** apps to control any hardware **over the Internet** or **directly using Bluetooth**.
You can easily build graphic interfaces for all your projects by simply dragging and dropping widgets, **right on your smartphone**.
Blynk is **the most popular IoT platform** used by design studios, makers, educators, and equipment vendors all over the world.

![Blynk Banner](https://github.com/blynkkk/blynkkk.github.io/blob/master/images/GithubBanner.jpg)

## Download

**Blynk App: 
[<img src="https://cdn.rawgit.com/simple-icons/simple-icons/develop/icons/googleplay.svg" width="18" height="18" /> Google Play](https://play.google.com/store/apps/details?id=cc.blynk) | 
[<img src="https://cdn.rawgit.com/simple-icons/simple-icons/develop/icons/apple.svg" width="18" height="18" /> App Store](https://itunes.apple.com/us/app/blynk-control-arduino-raspberry/id808760481?ls=1&mt=8)**

**Blynk [Server](https://github.com/blynkkk/blynk-server)**

## Documentation
Social: [Webpage](http://www.blynk.cc) / [Facebook](http://www.fb.com/blynkapp) / [Twitter](http://twitter.com/blynk_app) / [Kickstarter](https://www.kickstarter.com/projects/167134865/blynk-build-an-app-for-your-arduino-project-in-5-m/description)  
Help Center: http://help.blynk.cc  
Documentation: http://docs.blynk.cc/#blynk-firmware  
Community Forum: http://community.blynk.cc  
Examples Browser: http://examples.blynk.cc  
Blynk for Business: http://www.blynk.io

## Usage example

```lua
local Blynk = require("blynk.socket")

local blynk = Blynk.new("your_auth_token")

-- callback to run when V1 changes
blynk:on("V1", function(param)
  print("V1:", tonumber(param[1]), tonumber(param[2]))
end)

-- callback to run when cloud requests V2 value
blynk:on("readV2", function(param)
  blynk:virtualWrite(2, os.time())
end)

local sock = getSocketConnection() -- omitted
blynk:connect(sock)

while true do
  blynk:run()
end
```

You can run the [full example](examples/client.lua):

```sh
lua ./examples/client.lua <your_auth_token>
```

## Features
- Lua 5.1, Lua 5.2, Lua 5.3, LuaJIT support
- `virtualWrite`
- `syncVirtual`
- `setProperty`
- `notify`
- `logEvent`
- events: `Vn`, `readVn`, `connected`, `disconnected`
- `TCP` and secure `TLS/SSL` connection support
- can run on embedded hardware, like `NodeMCU` or `OpenWrt`

## OpenWrt instructions

```sh
opkg update
opkg install lua luasocket luasec
# openssl is needed for wget to handle https://
opkg install wget openssl-util libustream-openssl

# Get blynk-library-lua from github
cd /root
wget --no-check-certificate -qO- https://github.com/vshymanskyy/blynk-library-lua/archive/v0.1.3.tar.gz | tar xvz
cd blynk-library-lua-0.1.3

# Run it
lua ./examples/client.lua <your_auth_token>
```

## NodeMCU instructions

It is very easy to get it running on NodeMCU (or any other `ESP8266`/`ESP32`-based device):
- Get the latest [nodemcu-firmware](https://github.com/nodemcu/nodemcu-firmware) running on your device.  
  You can use their [online build service](https://nodemcu-build.com/).  
  It is recommended to include `encoder`, `TLS/SSL` modules.
- Edit `nodemcu.lua` example (put your `auth token` and wifi credentials)
- Use `nodemcu-tool` or any other method to transfer lua files to the device.  
  **Note:** the NodeMCU filesystem is "flat" (folders not supported), but it handles the `/` symbol nicely.  
  Be sure to preserve the relative path when copying files:
    ```sh
    nodemcu-tool upload -mck ./blynk.lua ./blynk/pipe.lua ./blynk/nodemcu.lua
    nodemcu-tool upload ./examples/nodemcu.lua -n init.lua
    ```
- Open device terminal and run `dofile("init.lua")`
- `blynk` object is global, so you can call it from the interactive console:
    ```lua
    blynk:virtualWrite(1, tmr.time())
    ```

## Ubuntu/Linux instructions

```sh
sudo apt-get install lua5.3 lua-sec lua-socket
```

## Bonus

The `Timer` is included for demonstration purposes.  
Here are also some handy functions:

```lua
local function millis()
  return math.floor(socket.gettime()*1000)
end

local function delay(msec)
  return socket.sleep(msec/1000)
end
```
__________

### Implementations for other platforms
* [Arduino](https://github.com/blynkkk/blynk-library)
* [Particle](https://github.com/vshymanskyy/blynk-library-spark)
* [Node.js, Espruino, Browsers](https://github.com/vshymanskyy/blynk-library-js)
* [Python, MicroPython](https://github.com/vshymanskyy/blynk-library-python)
* [OpenWrt packages](https://github.com/vshymanskyy/blynk-library-openwrt)
* [MBED](https://developer.mbed.org/users/vshymanskyy/code/Blynk/)
* [Node-RED](https://www.npmjs.com/package/node-red-contrib-blynk-ws)
* [LabVIEW](https://github.com/juncaofish/NI-LabVIEWInterfaceforBlynk)
* [C#](https://github.com/sverrefroy/BlynkLibrary)

### License
This project is released under The MIT License (MIT)

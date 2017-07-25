---
layout: post
title: Package Electron Application for OSx, Linux and Windows
---

I have been working on a small electron app. [Electron](https://electron.atom.io/) allows developers to build cross platform desktop app with platform agnostic technologies such as HTML, JavaScript and CSS.

I got the app working while developing very easily. Now I need to figure out how I can package the application so that it can be installed on Mac, Linux and Windows machines.
I found that there are two tools available to package electron app.

* [Electron-builder](https://github.com/electron-userland/electron-builder)
* [Electron-packager](https://github.com/electron-userland/electron-packager)

After doing bit of research I decided to use electron-builder as it has built-in support for Code signing and auto update.
I have tried this out on OSx and Windows operating systems.

You will start by adding electron-builder as a dev dependency.
```
$ npm install electron-builder --save-dev
```
After that create ```build``` directory under root of your project. Then save a ```background.png``` (macOS DMG background), ```icon.icns``` (macOS app icon) and ```icon.ico``` (Windows app icon) into it. The Linux icon set will be generated automatically based on the macOS.

Once you do that you will have to add following in your ```package.json``` file.
```
"build": {
    "appId": "your.app.id",
    "dmg": {
      "contents": [
        {
          "x": 240,
          "y": 150,
          "type": "link",
          "path": "/Applications"
        }
      ]
    },
    "win": {
      "target": "NSIS",
      "icon": "build/icon.ico"
    }
  },
```

Also add following into ```script``` section of  ```package.json``` file.
```
"postinstall": "electron-builder install-app-deps",
"pack": "electron-builder --dir",
"dist": "electron-builder"
```

And that should be it. To generate your installation package do following.
```
$ npm install
$ npm run dist
```

That will create ```dist``` directory under root of your application directory and put the appropriate installer depending on your operating system in a subfolder.

Hope this simple post helps you. This feature shows the true cross platform nature of electron and how easy it is to package and distribute your application in an installer.

-- Lav

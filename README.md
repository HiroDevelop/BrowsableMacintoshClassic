# Browsable Macintosh Classic

This project was originally located at https://github.com/jsdf/pce . Browsable Macintosh Classic is an attempt to fix the issue about save your valuable program data over the browser.

## How to install it on Ubuntu:

First install Apache Server

Install the depencency file:
```bash
sudo apt update
sudo apt install nodejs
sudo apt install npm
npm init <--- click enter on the questions that may appear
npm install --save pcejs-macplus pcejs-util
add some js index.js

var macplus = require('pcejs-macplus')
var utils = require('pcejs-util')
 
// add a loading progress bar. not required, but good ux
var loadingStatus = utils.loadingStatus(document.querySelector('.pcejs-loading-status'))
 
macplus({
  'arguments': ['-c','pce-config.cfg','-r'],
  autoloadFiles: [
    'macplus-pcex.rom',
    'mac-plus.rom',
    'hd1.qed',
    'pce-config.cfg',
  ],
  print: console.log.bind(console),
  printErr: console.warn.bind(console),
  canvas: document.querySelector('.pcejs-canvas'),
  monitorRunDependencies: function (remainingDependencies) {
    loadingStatus.update(remainingDependencies)
  },
})
```
add some html index.html

```bash
<!DOCTYPE html>
<html>
  <head>
    <style type="text/css">
    .pcejs {
      margin-left: auto;
      margin-right: auto;
      text-align: center;
      font-family: sans-serif;
      /* the canvas *must not* have any border or padding, or mouse coords will be wrong */
      border: 0px none;
      padding: 0;
    }
    .pcejs-container { margin-top: 32px }
    /* macplus has mouse integration, so we can hide the host mouse */
    .pcejs-canvas { cursor: none }
    </style> 
  </head>
  <body>
    <div class="pcejs pcejs-container">
      <div class="pcejs pcejs-loading-status">Downloading...</div>
      <div class="pcejs">
        <canvas class="pcejs pcejs-canvas" oncontextmenu="event.preventDefault()"></canvas>
      </div>
    </div>
    <script type="text/javascript" src="bundle.js"></script> 
  </body>
</html>

curl -O https://jamesfriend.com.au/pce-js/dist/macplus-system.zip
unzip macplus-system.zip

cp node_modules/pcejs-macplus/macplus-pcex.rom ./macplus-pcex.rom
cp node_modules/pcejs-macplus/pce-macplus.wasm ./pce-macplus.wasm

npm install -g browserify
browserify index.js --ignore-missing > bundle.js <-- Execute it by opening a new terminal section with root power.
```

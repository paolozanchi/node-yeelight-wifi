# node-yeelight-wifi-extended

"node-yeelight-wifi-extended" is a Node.js lib for controlling via wifi connected **Yeelights**. This package is a fork of the original [node-yeelight-wifi](https://github.com/Bastl34/node-yeelight-wifi) which adds additional methods toi control the bulbs.

It works with the RGBW bulbs, white bulbs and ceiling lights.

You need to activate "Lan control" on your lights. (see [Troubleshooting](#Troubleshooting))

## Installation

```shell
npm install --save node-yeelight-wifi-extended
```


## Howto

### Yeelight Lookup

You can lookup for yeelights in your local network like this:

```js
const Lookup = require("node-yeelight-wifi-extended").Lookup;

let look = new Lookup();

look.on("detected", (light) => {
    console.log("new yeelight detected: id=", light.id, "name=", light.name);
});
```

### Methods

Once you have a `Yeelight` (`light`) object from a lookup you can use it like this:

#### `setPower(power, duration)`

Set the power of your Yeelight (`true` to turn on, `false` to turn off)

```js
light.setPower(true).then(() => {
    console.log("success");
}).catch((error => {
    console.log("failed",error);
}));
```

#### `setRGB(rgb, duration)`

Set the RGB value (r: 0-255, g: 0-255, b: 0-255)

```js
light.setRGB([255,255,0]).then(() => {
    console.log("success");
}).catch((error => {
    console.log("failed",error);
}));
```

#### `setHSV(hsv, duration)`

Set the HSV value (hue: 0-359, sat: 0-100, value/bright=0-100)

```js
light.setHSV([180,70,100]).then(() => {
    console.log("success");
}).catch((error => {
    console.log("failed",error);
}));
```

#### `setCT(hsv, duration)`

Set the color temp value (1700 ~ 6500)

```js
light.setCT(5000).then(() => {
    console.log("success");
}).catch((error => {
    console.log("failed",error);
}));
```

#### `setBright(brightness, duration)`

Set the brightness (1-100)

```js
light.setBright(80).then(() => {
    console.log("success");
}).catch((error => {
    console.log("failed",error);
}));
```

#### `updateState()`

If you want to request a state (color, bright, power) update

```js
light.updateState().then(() => {
    console.log("success");
    console.log(light.rgb);
}).catch((error => {
    console.log("failed", error);
}));

```

### Usable (public) Members of `Yeelight`-Class (read only)

* `light.power` (`true`/`false`)
* `light.type` (`"unknown"`,`"white"`,`"color"`)
* `light.bright` (`0-100`)
* `light.rgb` (example: `{r: 0, g: 0, b: 0}`)
* `light.hsb` (example: `{h: 0, s: 0, b: 0}`)

### Events

A `Yeelight` instance has some events:

```js

//socket connect and disconnect events
light.on("connected", () =>{ console.log("connected"); });
light.on("disconnected", () => { console.log("disconnected"); });

//if the color or power state has changed
light.on("stateUpdate", (light) => { console.log(light.rgb); });

//if something went wrong
light.on("failed", (error) => { console.log(error); });

```


### Examples

* see `examples/examples.js`

### Troubleshooting

* Yeelight is not detected?
  * enable "LAN Control" on your yeelight
  * https://www.yeelight.com/faqs/lan_control

## Licence

Copyright (c) Bastian Karge

MIT (see License.md)

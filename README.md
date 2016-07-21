# Homey nRF905 nodemodule
Homey app developers can use this nodemodule to communicate with a nRF905 module over 433MHz wired to a arduino. The arduino must be running the Homey-arduino-nRF905 library which can be found [here](https://github.com/athombv/homey-arduino-nrf905).

Currently it is only possible to receive data from a arduino device by first sending data to it. After sending data the driver switches to receive mode and waits 200ms for the arduino to respond back. After the 200ms have passed Homey returns to its default receive configuration. 

The module includes automatic acking of transmitted and received messages, which makes it easy to detect dropped messages. 

Examples are provided in the examples/ folder. 

## Requirements
- a nRF905 wired to a Arduino running the Homey-arduino-nRF905 library
- Homey running 0.9.0 or later

## Installation
Import the file `arduino_radio.js` to your existing Homey app project. 
## Usage
first, create a RadioArduino instance with a desired receive address:

```
var radio = new HomeyRadio({address: 0x50});
```
listen to incoming data events:
```
radio.on('payload', function(message) {
    console.log(message);
});
```
or send data to a arduino:
```
var payload = new Buffer(['h','e','l','l','o','\n']);
radio.send(0xAA, payload, function(err) {
    if(!err) {
        console.log("message acked!");
    } else {
        console.log("message not acked");
});
```
### Constructors
- `HomeyArduino(opts)`
    * ***Description:*** Constructor which spawns a HomeyArduino object
    * ***Parameters:***
        * ***opts.address:*** receive address of Homey 

### Methods
- `HomeyArduino.send(address, data, callback)`
    * ***returning:***  -
    * ***Description:***  method to send data to a arduino device
    * ***Parameters:***
        * ***address:*** receive address of the nRF905 module
        * ***data:*** Buffer with payload data ***Max 8 bytes***
        * ***callback:*** function(err) which get called on a timeout or ack
### Events
- `HomeyArduino.on('payload', function(message){})`
    * ***Description:***  Method to send data to a arduino device
    * ***Parameters:***
        * ***message.srcAddr:*** address of arduino
        * ***message.destAddr:*** address of homey
        * ***message.type:*** message type, 'data'
        * ***message.sequenceNr:*** sequence number
        * ***message.payload:*** Buffer including the received data 
        * ***message.crc:*** crc checksum


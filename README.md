# GPSD Client

PHP Client to retrieve informations from a GPSD service.
More informations about GPSD: http://catb.org/gpsd/

## Installation

```
composer require nykopol/gpsd-client 1.*
```

## Usage

Basic usage :

```php
$client = new Client(); // new client for localhost on port 2947
$client->connect(); // Initiate socket with the service
$client->watch();   // Tell the service to start report event
$infos = $client->getNext('TPV'); // Get the next message of class TPV ()
```

## Documentation

### __construct($host = 'localhost', $port = 2947)

The class instantiation accept two parameters:
*$host* `string` (default: `localhost`) the host we want to listen to
*$port* `integer` (default: `2947`) the socket port


### connect()

This method connect the socket to the service. The returned value is the string
returned by the service identifying itself.
ex.: `{"class":"VERSION","release":"2.93","rev":"2010-03-30T12:18:17",
"proto_major":3,"proto_minor":2}`


### watch($enable = true, $format = 'json')

The watch method tell the service to start report sensors events. This method
accept two parameters:
*$enable* `bool` (default: `true`) Tell the service to start or stop watching.
*$format* `string` (default: `json`) Set the desired format for the returned datas


### getNext($class, $buffer = false, $blocking = true)

Return the next message of the given class. Current service class are `WATCH`,
`DEVICES`, `DEVICE`, `TPV`, `AIS`.
The method accept the tree parameters:
*$class* `string` The wanted class name.
*$buffer* `bool` (default: `false`) Once the device synced, the service is
generaly reporting one message per second. This parameter specify if we want the
next message from the buffer or the next message in the time.
*$blocking* `bool` (default: `true`) Wait for the next message or return null if
there is no new message. This option is relevant only if the buffer is enabled.

For exemple, if you are connected to the service since 1 minute, and that the
sensor is synced, the next message from the buffer is proably old of about 1 minute.
While requesting the next message in time will flush the buffer then wait for
the next message of the desired class.

The returned value is the raw string returned by the service. So if you asked for
json format with the `watch()` method, the returne value will look like :
`{"class":"TPV","tag":"MID2","time":"2010-04...`


### disconnect()

Disconnect from the service.

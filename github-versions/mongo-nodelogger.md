# nodelogger: a piped logger for apache -- writes geolocation and access log data to mongodb 

> Wes Freeman
> wes@skeweredrook.com
> @wefreema
> http://github.com/wfreeman

## A Problem 

* Traditional rotatelogs support from apache requires a process for each virtual host.
* An apache configuration for a shared server can have hundreds of virtualhost directives.
* Multiplied together = hundreds of processes just for logging.
* Files need deleting/maintaining. Somewhat annoying to open them for viewing.

## A Solution #

* There are many solutions to this problem: eg. vlogger (virtual-logger).
* However, partly as an exercise in using MongoDB, and also Node.js, I wrote my own.
* As a bonus, Node.js contains a wrapper for GeoIP from MaxMind (an IP->location service).

## High level design
* Listen on standard input (Apache sends each log record in a single line).
* Split line using a space delimiter.
* Look up location information based on the IP from the record.
* Connect to Mongo, insert a simple document with the log & location information.

## Listening on standard input in Node
``` javascript
var stdin = process.stdin;
stdin.resume(); 
stdin.on('data', function (chunk) { 
  var line = chunk.toString().replace(/\n/, '\\n');
  console.log('stdin: ' + line);
  /* do stuff here */
}).on('end', function () { 
  console.log('stdin:closed exiting');
});
```

## Getting Location from IP
```javascript
var GeoIP = require('geoip');
var City = GeoIP.City;
var city = new City('./GeoLiteCity.dat');
...
city_obj = city.lookupSync(ipstr),
lat = city_obj.latitude;
lon = city_obj.longitude;
```

## MongoDB-node-native driver
```javascript
var MongoDB = require('mongodb'),
  Db = MongoDB.Db,
  Server = MongoDB.Server;
db = new Db('nodelogs', 
  new Server(host, port, {}), {native_parser: true});
```

## Inserting into Mongo
```javascript
db.open(function (err, db) {
  db.collection(vhost, function (err, collection) {      
    collection.insert({
      'vhost': vhost,
      'ip': line_arr[1],
      'url': line_arr[7],
      'dt': new Date(),
      'lat': lat,
      'lon': lon
    });
    db.close();
  });
});
```

## Putting it all together
* [https://github.com/wfreeman/nodelogger/blob/master/nodelogger.js](https://github.com/wfreeman/nodelogger/blob/master/nodelogger.js)

## Future plans
* Error log capability
* Node.js--sockets-based log viewer
> * Searching
> * Tail -f capability
> * Heatmap with world map view; shows where your visitors are coming from

# MongoDB in a shared hosting environment #

Wes Freeman   
wes@skeweredrook.com  
@wefreema  
http://github.com/wfreeman  

## Topics for shared hosts #

* Security
* Access control
* Speculation on MongoHQ's inner workings
* Rock Mongo (PHP-based MongoDB client/query browser)
* Wishlist

## Shared host security #

* Mongodb is not secure out of the box!
* Documentation recommends isolated "trusted" environment--configure your firewall/network accordingly.

## Access control #

* Once you are authenticated to a mongod process, you can access any database within the service.
  * This is bad from a shared host perspective.
* The current version of mongo requires that you configure separate services on alternative ports for each user, for the best security.
* Note: Replica Set authentication is separate from user authentication.

## MongoHQ -- How do they have secure 16MB and 256MB non-dedicated services? 
* MongoHQ appears to have broken the show dbs/show collections functionality, and only lets you access your own db. 
  * It looks like they've hacked some better access control for shared mongo services. 
  * This is probably why their paid shared mongodb instances are running 1.6.5 instead of something more current.
* Recent News: did you know that they recently acquired their competitor, MongoMachine?

## Rock Mongo #

* Rock Mongo is to mongodb what phpMyAdmin is to mysql.
* It's a php-based mongodb client, for editing/querying data.
* Gives access to mongodb without opening mongodb ports, and doesn't require ssh access.
* Separate access control--you configure users for Rock Mongo separately.
* You probably want to put it behind SSL.

## Wishlist #

* More functional access control to allow finer-grained privileges.
* Control for memory usage limits.

!SLIDE 
## MongoDB in a shared hosting environment ##

> Wes Freeman 
>
> wes@skeweredrook.com
>
> @wefreema
>
> http://github.com/wfreeman
>

!SLIDE smbullets
### Things to worry about in particular for shared hosts: ###

* security
* access control
* memory usage

!SLIDE smbullets
### Shared host security ###

* vanilla mongodb is not secure!
* once you are authenticated to a mongod process, you can access any database within the service (this is bad)
* the current version of mongo requires that you configure separate services on alternative ports for each user, for the best security.

!SLIDE smbullets
### access control ###

*  

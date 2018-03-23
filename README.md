## Squidward Squid Container
Container to run squid proxy cache for rpm, deb, and other packages.

To deploy
`docker pull koffin/squidward` and then  
`docker run -itd --name squid --publish 3128:3128 --volume </HOST_MACHINE/PATH/CACHE>:/var/cache/squid squid`  
An example:  
`docker run -itd --name squid --publish 3128:3128 --volume /private/var/spool/squid:/var/cache/squid squid`

OR
```
docker run --name squid -d --restart=always \
  --publish 3128:3128 \
  --volume /private/var/spool/squid:/var/cache/squid \
  koffin/squidward
```

Point any and all clients you want to use the cache proxy to the server's IP address that is running docker and append port 3128 to it.  
An example:  
`http://192.168.1.1:3128`  

On an a linux machine you can temporarily add it by exporting the command to your environment:  

`export {http,https,ftp}_proxy="http://PROXY_SERVER:PORT"` 
To remove them:
`unset {http,https,ftp}_proxy`

To make this permanent on **Ubuntu**, set up something in your `/etc/environment` file:  
```
http_proxy=http://10.1.1.1:3128/  
https_proxy=http://10.1.1.1:3128/  
ftp_proxy=http://10.1.1.1:3128/  
no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"  
HTTP_PROXY=http://10.1.1.1:3128/ 
HTTPS_PROXY=10.1.1.1:3128/  
FTP_PROXY=10.1.1.1:3128/  
NO_PROXY="localhost,127.0.0.1,localaddress,.localdomain.com" 
Acquire::http::proxy "http://10.1.1.1:3128";
Acquire::ftp::proxy "http://10.1.1.1:3128";
Acquire::https::proxy "http://10.1.1.1:3128";
```  


---  
Some details about the squid.conf:  
The maximum size of the overall cache to be retained is 10GB.  
`cache_dir aufs /var/cache/squid 10240 16 256`  

The maximum size object that can be saved is 8GB.  
`maximum_object_size 8192 MB`  

The minimum size object that can be saved is 0MB. 

The following objects are cached for 69 days:  
deb, rpm, pkg, tgz, tar, and iso files.


# squidward_container
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

On linux machines you can temporarily add it by exporting the command to your environment:  

`export {http,https,ftp}_proxy="http://PROXY_SERVER:PORT"` 
To remove them:
`unset {http,https,ftp}_proxy`

To make this more permanent, set up something in your `~/.bashrc`:  
```
# Set Proxy
function setproxy() {
    export {http,https,ftp}_proxy="http://PROXY_SERVER:PORT"
}

# Unset Proxy
function unsetproxy() {
    unset {http,https,ftp}_proxy
}
```  

reload `~/.bashrc`

---  
Some details about the squid.conf:  
The maximum size of the overall cache to be retained is 10GB.  
`cache_dir aufs /var/cache/squid 10240 16 256`  

The maximum size object that can be saved is 8GB.  
`maximum_object_size 8192 MB`  

The minimum size object that can be saved is 0MB. 

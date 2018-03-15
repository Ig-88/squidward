# squidward_container
Container to run squid proxy for rpm, deb, and other packages

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

# Tag `latest`
This tag refers to the latest version of bird and debian available when I built images

# Tag `2.0.4`
This image is based on the 2.0.4 version of [BIRD](https://bird.network.cz) and build from debian

# Notes

This work is based on [pierky/dockerfiles](https://github.com/pierky/dockerfiles) and [pierky/bird](https://hub.docker.com/r/pierky/bird)

These are multi-stage build to reduce disk usage thanks to @jptazzo https://intro-2019-04.container.training/#196

You may also want to try alpine instead of debian -Left as an exercise for the reader-

These images are build with the commands : 
```
docker build -t afenioux/bird --build-arg BIRDV=2.0.4 .
docker build -t afenioux/bird:2.0.4 --build-arg BIRDV=2.0.4 .
```

# Bird configuration

If you run with this option : `docker run -d -p 179:179 -v /mnt/flash/docker/bird:/etc/bird:rw --memory 512m --memory-swap 512m --cpus 1.1 afenioux/bird`

The host's file at /somewhere/bird/bird.conf is used as the configuration file for BIRD (/etc/bird/bird.conf in the containter) and can be edited
on the host.

Logs are available with `docker logs <container_id> is you set `log stderr all;` in bird.conf, but you may write in a file like /etc/bird/bird.logs

see [BIRD 2 documentation](https://bird.network.cz/?get_doc&f=bird.html&v=20)
and [BIRD 2 configuration example](https://github.com/BIRD/bird/blob/master/doc/bird.conf.example)
for more details.

# Example
```
#docker run -d -p179:179 -v /mnt/flash/docker/bird:/etc/bird:rw --memory 512m --memory-swap 512m --cpus 1.1 afenioux/bird
f7944ae0253e3742f9de7d8e06ca4d468ac65f7caf1ce36bf1fc75d454e3766a

# docker logs f794
2019-04-29 13:00:45.527 <INFO> Started

# docker exec -it f794 birdc "configure soft"
BIRD 2.0.4 ready.
Reading configuration from /etc/bird/bird.conf
Reconfigured
```

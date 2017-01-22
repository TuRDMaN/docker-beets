# TuRDMaN/docker-irs
[![](https://images.microbadger.com/badges/version/linuxserver/beets.svg)](https://microbadger.com/images/linuxserver/beets "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/linuxserver/beets.svg)](http://microbadger.com/images/linuxserver/beets "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/beets.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/linuxserver/beets.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io/linuxserver-beets)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io/job/linuxserver-beets/)
[hub]: https://hub.docker.com/r/linuxserver/beets/

## Usage

```
docker create \
--name=irs \
-v <path to data>:/config \
-e PGID=<gid> -e PUID=<uid>  \
-p 1234:1234 \
TuRDMaN/docker-irs
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 8337` - the port(s)
* `-v /config` - Configuration files
* `-v /music` - Music library location
* `-v /downloads` - Non-processed music
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it beets /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 

Edit the config file in /config

To edit the config from within the container use `beet config -e`

For a command prompt as user abc `docker exec -it -u abc beets bash`

See [Beets][beetsurl] for more info.

Contains [beets-copyartifacts](https://github.com/sbarakat/beets-copyartifacts) plugin, [configuration details](https://github.com/sbarakat/beets-copyartifacts#configuration)

## Info

* To monitor the logs of the container in realtime `docker logs -f beets`.

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' beets`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/beets`


## Versions

+ **16.01.17:** Add packages required for replaygain.
+ **24.12.16:** Add [beets-copyartifacts](https://github.com/sbarakat/beets-copyartifacts) plugin.
+ **07.12.16:** Edit cmake options for chromaprint, should now build and install fpcalc, add gstreamer lib
+ **14.10.16:** Add version layer information.
+ **01.10.16:** Add nano and editor variable -
to allow editing of the config from the container command line.
+ **30.09.16:** Fix umask.
+ **24.09.16:** Rebase to alpine linux.
+ **10.09.16:** Add layer badges to README.
+ **05.01.16:** Change ffpmeg repository, other version crashes container
+ **06.11.15:** Initial Release
+ **29.11.15:** Take out term setting, causing issues with key entry for some users

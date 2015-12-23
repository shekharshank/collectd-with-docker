# collectd with docker image

## Description

collectd is a daemon which collects system performance statistics periodically
and provides mechanisms to store the values in a variety of ways, for example 
in RRD files.

This image modifies the original image - fr3nd/docker-collectd to support collection of docker metrics by integrating the collectd plugin lebauce/docker-collectd-plugin. The configuration file - collectd.conf can be modified according to the user needs, by
default it collects cpu and docker metrics and publishes to a kafka broker on a host named benchmark1.

## How to use this image

```
docker build collectd-with-docker

docker run --privileged \
  --hostname benchmark2 --add-host benchmark1:X.X.X.X \
  -v /proc:/mnt/proc:ro -v /var/run/docker.sock:/var/run/docker.sock \
  -v /sys/fs/cgroup/:/sys/fs/cgroup:ro <IMAGE_NAME>
```

## FAQ

### Do you need to run the container as privileged?

Yes. Collectd needs access to the parent host's /proc filesystem to get
statistics. It's possible to run collectd without passing the parent host's
/proc filesystem without running the container as privileged, but the metrics
would not be acurate.


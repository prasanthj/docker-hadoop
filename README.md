Apache Hadoop 2.5.0 Docker image

This is Hadoop 2.5.0 Docker image mostly adapted from https://github.com/sequenceiq/hadoop-docker but for Ubuntu (trusty).

# Running on Mac OS X

This step is required only for Mac OS X as docker is not natively supported in Mac OS X. To run docker on Mac OS X we need Boot2Docker.

## Setting up docker

* Install Boot2Docker from [here].
* After installing, from terminal, run `boot2docker init` to initialize boot2docker.
* Run `boot2docker start` to start boot2docker and export `DOCKER_HOST` as shown at the end of command. It usually will be `export DOCKER_HOST=tcp://192.168.59.103:2375`
* After exporting `DOCKER_HOST` we can run docker commands.

*NOTE:* docker 1.3.0 versions require --tls to be passed to all docker command

# Pull the image
```
docker --tls pull prasanthj/hadoop-2.5.0
```

# Build the image

* To build the hadoop docker image locally from Dockerfile, first checkout source using
`git clone TBD`
* Change to hadoop directory `cd hadoop`

```
docker --tls build  -t prasanthj/hadoop-2.5.0 .
```
# Start a container

In order to use the Docker image you have just build or pulled use:

```
docker --tls run -i -t prasanthj/hadoop-2.5.0 /etc/bootstrap.sh -bash
```

## Testing

You can run one of the stock examples:

```
cd $HADOOP_PREFIX
# run the mapreduce
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.1.jar grep input output 'dfs[a-z.]+'

# check the output
bin/hdfs dfs -cat output/*
```

[here]:https://github.com/boot2docker/osx-installer/releases

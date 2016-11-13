# Apache Hadoop 2 Docker image


This is Hadoop 2 Docker image mostly adapted from https://github.com/sequenceiq/hadoop-docker but for Ubuntu (trusty).

## Current Version
* Apache Hadoop 2.6.0

## Running on Mac OS X

This step is required only for Mac OS X as docker is not natively supported in Mac OS X. To run docker on Mac OS X we need Boot2Docker.

### Setting up docker

* Install Boot2Docker from [here].
* After installing, from terminal, run `boot2docker init` to initialize boot2docker.
* Run `boot2docker start` to start boot2docker and export `DOCKER_HOST` and `DOCKER_CERT_PATH` as shown at the end of command.
* After exporting `DOCKER_HOST` and `DOCKER_CERT_PATH` we can run docker commands.

*NOTE:* docker 1.3.0 versions require --tls to be passed to all docker command

## Pull the image
You can either pull the image that is already pre-built from Docker hub or build the image locally (refer next section)
```
docker --tls pull prasanthj/docker-hadoop
```

## Build the image
If you do not want to pull the image from Docker hub, you can build it locally using the following steps
* To build the hadoop docker image locally from Dockerfile, first checkout source using
`git clone https://github.com/prasanthj/docker-hadoop.git`
* Change to docker-hadoop directory `cd docker-hadoop`

```
docker --tls build  -t local-hadoop-2.6.0 .
```
## Start a container

In order to use the Docker image you have just build or pulled use:

```
docker --tls run -i -t local-hadoop-2.6.0 /etc/bootstrap.sh -bash
```

### Testing

You can run one of the stock examples:

```
# run the mapreduce
$HADOOP_PREFIX/bin/hadoop jar $HADOOP_PREFIX/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep input output 'dfs[a-z.]+'

# check the output
$HADOOP_PREFIX/bin/hdfs dfs -cat output/*
```

## Viewing Web UI
If you are running docker using Boot2Docker then do the following steps

 * Setup routing on the host machine (Mac OS X) using the following
   command `sudo route add -net 172.17.0.0/16 192.168.59.103`
_NOTE_: 172.17.0.X is usually the ipaddress of docker container. 192.168.59.103 is the ipaddress exported in `DOCKER_HOST`

 * Get containers IP address
	* To get containers IP address we need CONTAINER_ID. To get container id use the following command which should list all running containers and its ID
	`docker --tls ps`
	* Use the following command to get containers IP address (where CONTAINER_ID is the container id of local-hadoop-2.6.0 (or prasanthj/docker-hadoop if pulled from docker hub) image)
	`docker --tls inspect -f=“{{.NetworkSettings.IPAddress}}” CONTAINER_ID`

 * Launch a web browser and type `http://<container-ip-address>:8088` to view hadoop cluster web UI.

[here]:https://github.com/boot2docker/osx-installer/releases

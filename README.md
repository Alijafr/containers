# containers
Contains some docker containers that includes Ubuntu 18, CUDA 10.1, PX4.
# Note
if you dont have Nvidia gpu, remove $DOCKER_OPTS (line 103) in run_docker.sh
# Usage
First, you need to install docker. You can use the `script/setup_docker.sh` script for that.
To pull docker image and run it, use
```sh
cd scripts
./run_docker.sh px4
```
`px4` is a name of your choice of the container.

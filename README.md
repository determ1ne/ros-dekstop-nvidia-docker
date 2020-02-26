# ros-dekstop-nvidia-docker

A simple image including a full ROS desktop environment.

## Prerequisite

- Docker with nvidia container runtime
- X11 Server

## Building

```shell
docker build -t <your_image_name> .
```

For users in Mainland China, please use the `Dockerfile` in folders with suffix `-china`, which uses mirrors to accelerate the build. However, Github access is still needed, you may need to pass a proxy for build.

```shell
docker build --build-arg http_proxy=<your_http_proxy> --build-arg https_proxy=<your_https_proxy> --network=host -t <your_image_name> .
```

## Usage

- Install `nvidia-container-toolkit`.

- For the first time before starting a container, setup X11 socket and X11 auth files:

```shell
XAUTH=/tmp/.docker.xauth
touch $XAUTH
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f $XAUTH nmerge -
```

- Run a container with interactive bash:

```shell
docker run -it \
    --gpus all \
    --env "DISPLAY" \
    --volume /tmp/.X11-unix:/tmp/.X11-unix:rw \
    --volume /tmp/.docker.xauth:/tmp/.docker.xauth:rw \
    --env "XAUTHORITY=/tmp/.docker.xauth" \
    <your_image_name> bash
```
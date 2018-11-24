# fastai_docker
nvidia-docker (GPU) environment for fastai 2018 course. 

Clone with submodules: `git clone  --recurse-submodules https://github.com/KoppeKTop/fastai_docker.git`

Persistent data stored in ./data folder, it mounts into `user_data` folder inside main directory.

Jupyter password can be set via JUPYTER_PASSWORD variable. 

## prepare

1. Install Docker: 

`curl -sSL https://get.docker.com/ | sh`

2. Install docker-compose:

`sudo curl -L https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose`

3. Install [nvidia-docker2](https://devblogs.nvidia.com/gpu-containers-runtime/) 

To run container with GPU available from docker-compose one need to set `nvidia` as a default runtime in `/etc/docker/daemon.json` file, so it looks like this:

```
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    },
    "default-runtime": "nvidia"
}
```

## run
`docker-compose up --build -d -f docker-compose.build.yml`

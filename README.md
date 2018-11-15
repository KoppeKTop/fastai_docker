# fastai_docker
nvidia-docker (GPU) environment for fastai 2018 course. 

Clone with submodules: `git clone  --recurse-submodules https://github.com/KoppeKTop/fastai_docker.git`

Persistent data stored in ./data folder, it mounts into `user_data` folder inside main directory.

Jupyter password can be set via JUPYTER_PASSWORD variable. 

# run
`docker-compose up --build -d -f docker-compose.build.yml`

# fastai_docker
Docker environment for fastai 2018 course. 
Don't forget to clone fastai/fastai submodule to have latest version of sources. 
Persistent data stored in ./data folder, it mounts into `user_data` folder inside main directory.
Jupyter password can be set via JUPYTER_PASSWORD variable. 

# build
`docker-compose build`

# run
`docker-compose up -d`

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

## nginx in front
This container is target on localhost usage, to make it public available - you should use SSL termination.
For this I prefer nginx + letsencrypt. So my nginx config looks like this:

```
server {
        listen 443 ssl;
        listen [::]:443 ssl;

        location ^~ /.well-known/acme-challenge/ {
                default_type "text/plain";
                root /var/www/letsencrypt;
        }

        ssl_certificate /etc/letsencrypt/live/mydomain.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/mydomain.com/privkey.pem;
        ssl_trusted_certificate /etc/letsencrypt/live/mydomain.com/fullchain.pem;
        client_max_body_size 1024M;
        client_body_buffer_size 4M;

ssl_session_timeout 1d;
ssl_session_cache shared:SSL:50m;
ssl_session_tickets off;

ssl_protocols TLSv1.2;
ssl_ciphers EECDH+AESGCM:EECDH+AES;
ssl_ecdh_curve secp384r1;
ssl_prefer_server_ciphers on;

ssl_stapling on;
ssl_stapling_verify on;

add_header Strict-Transport-Security "max-age=15768000; includeSubdomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;

        access_log            /var/log/nginx/jupyter.access.log;
        error_log            /var/log/nginx/jupyter.error.log;

        server_name mydomain.com;

        location / {
                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto $scheme;

                proxy_pass          http://127.0.0.1:8888;
                proxy_read_timeout  90;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";
                proxy_set_header Origin "";
        }
}

server {
        listen 80;
        listen [::]:80;
        location ^~ /.well-known/acme-challenge/ {
                default_type "text/plain";
                root /var/www/letsencrypt;
        }
        server_name mydomain.com;
        return 301 https://$host$request_uri;
        access_log            /var/log/nginx/jupyter.access.log;
        error_log            /var/log/nginx/jupyter.error.log;
}

```

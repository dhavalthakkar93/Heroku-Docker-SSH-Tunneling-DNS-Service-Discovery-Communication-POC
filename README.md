# Heroku-Docker-Communication 

This proof of concept app is intended to demonstrate Nginx Reverse Proxy using the Docker conatainer and SSH Tunneling of Docker Container in Heroku Private Spaces.  

First clone the code

```
git clone https://github.com/dhavalthakkar93/Heroku-Docker-Communication-POC
```

Create Heroku App

```
heroku create -a docker-ssh-tunneling-poc

Creating app... done, ⬢ docker-ssh-tunneling-poc
https://docker-ssh-tunneling-poc.herokuapp.com/ | https://git.heroku.com/docker-ssh-tunneling-poc.git
```

# Build Images 

- `docker build -t back backend-container/.`
- `docker build -t front frontend-container/.`
- `docker build -t web web-container-nginx/.`
- `docker build -t debug .`

# Deploy Images to Heroku

- `heroku container:push back -a <APP_NAME>`
-  `heroku container:release back -a <APP_NAME>`

- `heroku container:push front -a <APP_NAME>`
- `heroku container:release front -a <APP_NAME>`

- `heroku container:push web -a <APP_NAME>`
- `heroku container:release web -a <APP_NAME>`

- `heroku container:push debug -a <APP_NAME>`
- `heroku container:release debug -a <APP_NAME>`

# SSH into debug Dyno

```
heroku ps:exec --dyno=debug.1 -a <APP_NAME>

Establishing credentials... done
Connecting to debug.1 on ⬢ docker-ssh-tunneling-poc... 
~ $ 
```

# CURL to Frontend & Backend Container

```
~ $ curl -L front.docker-ssh-tunneling-poc.app.localspace:9393
Hello World From Frontend

~ $ curl -L back.docker-ssh-tunneling-poc.app.localspace:4000
Hello World From Backend
```

Here `front` and `back` is dyno name (AKA Docker Container tag) `docker-ssh-tunneling-poc` is `Heroku Application Name`

# PORT Configuration 

We can set the static port for the each process at application level, In Heroku all ports on dyno can be reachable from inside the space so no need of `EXPOSE` in `Dockerfile`.

Here in this POC frontend has port `9393` and backend has port `4000` (check server.js).



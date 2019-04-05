# Chatopia project Docker configuration

## Running locally
From your local root directory of this repo:

```bash
./development-config
```

This just creates a symbolic link from `docker-compose.override.yml` (which docker-compose knows to look for) to `docker-compose.dev.override.yml`, which contains a couple changes necessary to run in a local development environment. 

```bash
docker-compose pull  # If you've pulled images before this will update them.
docker-compose up
```

Adding the `--detach` option to this command will run the containers in the background and not spit out logs.

To try out your new server:

* Go to <https://riot.im/app/#/login>
* Click _Change_ in the upper right to not use `matrix.org`
* Enter <http://localhost:8008> for _Home Server URL_
* Click Next
* Log in with (literally) `user` / `password`

Click on the _Testing Room_ in the left sidebar. Then you can interact with the bots.

You can change the username and password of the test account in the `docker-compose.dev.override.yml` file, as well as the IRC channel to join.

## Developing locally
Clone the component(s) you want to work on:
#### Synapse server (which implements the Matrix chat API)
```bash
git clone https://github.com/chatopia/synapse.git
```
#### IRC<->Matrix bridge
```bash
git clone https://github.com/chatopia/matrix-appservice-irc.git
```
#### IRC bots
```bash
git clone git@sneakyfrog.com:bots.git
```

Edit `docker-compose.dev.override.yml` and uncomment the build context(s) for the component(s) you're working on. Change the directory if necessary to match your workspace structure.

At this point `docker-compose up` should build your local version if it contains any changes.

To publish your changes, run `docker-compose push`.

## Deployment
### On an existing DigitalOcean instance
```bash
ssh root@chat.sneakyfrog.com  # or IP address
cd docker-config
docker-compose stop
docker-compose up --detach
```

### On a new server instance
From the Chatopia dashboard on <https://digitalocean.com>, create a new droplet. You should only need to change these settings from the defaults:

* _Image:_ Docker image (found in the Marketplace tab)
* _Plan:_ Standard, with tiniest VM ($5/month)
* _Region:_ San Francisco 2 (necessary to mount the persistent data volume)
* _SSH Keys:_ include all of them

```bash
ssh root@(new-droplet-IP)
```

```bash
git clone git@github.com:chatopia/chatopia-docker.git
cd chatopia-docker
./production-config
docker-compose up --detach
```

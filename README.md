# docker-config

## Running locally
From your local root directory of this repo:

```bash
./development-config
```

This will create a symbolic link from `docker-compose.override.yml` (which docker-compose knows to look for) to the dev override file, which contains a couple changes necessary to run locally. 

```bash
docker-compose up
```

When its database creation dust settles, you should be able to connect to Synapse at `http://localhost:8008` using Riot or another client and create an account. Then you can `/join #irc_#spot` or any other room that the bots hang out in (`#irc_#botopia` obviously, or `#irc_#botsbotsbots` for the whole crowd). Adding the `--detach` option to this command will run the containers in the background and not spit out logs.

## Developing locally
Edit `docker-compose.dev.override.yml` and uncomment the build context(s) for the component(s) you're working on. Change the directory if necessary to match your workspace structure.

At this point `docker-compose up` should build your local version if it contains any changes.

To publish your changes, run `docker-compose push`.

## Deployment
### On an existing instance
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

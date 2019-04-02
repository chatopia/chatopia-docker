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

Adding the `--detach` option to this command will run the containers in the background and not spit out logs.

To try out your new server:

* Go to <https://riot.im/app/#/login>
* Click _Create Account_
* Click on _Advanced / Other_
* Enter <http://localhost:8008> for _Home Server URL_
* Click Next
* Pick a username and password
* Click _Register_
* Hit _Continue_ to ignore the email warning

There will be an annoying pause for now and then an error creating room message until we fix <https://www.pivotaltracker.com/story/show/165073247>. Just click OK and then you can join the _Empty Room_ on the left.
Type
```irc
/join #_test
```
That should take you to the test room, and then you can interact with the bots.


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

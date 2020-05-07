# QUICKSTART.md

## Step 1 - setup & clone
Setup the account for automated `sudo` privileges, install the `git` program, and clone the repository.

```
echo "${USER} ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/010_${USER}-nopasswd
sudo apt install -qq -y git 
git clone http://github.com/dcmartin/open-horizon
```

## Step 2 - update & install
Run the update and upgrade processes for the system and then install the required tools; when complete, add the account to the `docker` group and reboot the machine.

```
sudo apt update -qq -y
sudo apt upgrade -qq -y
sudo apt install -qq -y build-essential net-tools jq curl apache2-utils gnupg2 pass docker-compose
sudo addgroup ${USER} docker
sudo reboot
```

## Step 3 - create exchange
Initialize the random number genertor, change to the directory and use `make` to build the _exchange_.

```
touch ~/.rnd
cd open-horizon
make exchange
```

## Step 4 - install horizon
Run the provided shell script to download `horizon`, `bluehorizon`, and `horizon-cli` packages and install.

```
sudo ./sh/get.horizon.sh
```

## Step 5 - login to hub.docker.com
Login to Docker (aka `hub.docker.com`); the `DOCKER_NAMESPACE` defaults to `USER` environment variable; 
override by setting the environment variable or creating a persistent file of the same name.

```
docker login
```

## Step 6 - build & run `hznmonitor` service
Build the containers for this service and run the service to browse the exchange and the services published using a browser on port `3094`

```
make hznmonitor
```

## Step 7 - build, push, and publish _all_ services
Build, push, and publish all the services

```
make services
```
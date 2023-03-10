# Setup Redis Server
1. Mac Apple Silicon
2. Linux


## Install Redis Server in Mac Apple Silicon

Open Terminal

```sh
# To install the redis
brew install redis

# To start the service
brew services start redis

# To stop the service
brew services stop redis

```


## Install Redis Server in Ubuntu Linux 20.04

Open Terminal

```sh
sudo apt update

sudo apt install redis-server

sudo systemctl enable redis.service
```

Ensure that the server is running using the systemctl start command:

```sh
sudo systemctl start redis.service
```

### Login in Redis CLI
```sh
redis-cli
```

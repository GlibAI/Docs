# Setup RabbitMq
1. Mac Apple Silicon
2. Linux


## Install RabbitMq in Mac Apple Silicon

Open Terminal

```sh
brew install rabbitmq
```


## Install RabbitMq in Ubuntu Linux 20.04

### Step 1: Install Erlang/OTP

```sh
sudo apt update
sudo apt install curl software-properties-common apt-transport-https lsb-release
curl -fsSL https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/erlang.gpg

echo "deb https://packages.erlang-solutions.com/ubuntu $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/erlang.list

sudo apt update
sudo apt install erlang
```

### Step 2: Add RabbitMQ Repository to Ubuntu

```sh
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.deb.sh | sudo bash
```

### Step 3: Install RabbitMQ Server Ubuntu 20.04

```sh
sudo apt update
sudo apt install rabbitmq-server
sudo systemctl enable rabbitmq-server
```

### Step 4: Start RabbitMQ Server

```sh
sudo systemctl start rabbitmq-server
```

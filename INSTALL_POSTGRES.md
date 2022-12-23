# Setup Postgres
1. Mac Apple Silicon
2. Linux


## Install Postgres in Mac Apple Silicon

Open Terminal

```sh
# brew install postgresql@{{version}}

brew install postgresql@13
```


## Install Postgress in Ubuntu Linux 20.04

Open Terminal

```sh
sudo apt update

sudo apt install postgresql postgresql-contrib

sudo systemctl enable postgresql.service
```

Ensure that the server is running using the systemctl start command:

```sh
sudo systemctl start postgresql.service
```

### Login in PSQL
```sh
psql postgres (For Mac) / sudo -u postgres psql (For Ubuntu 20.04)
```

### Using PostgreSQL Roles and Databases
```sh
postgres=# CREATE DATABASE databasename;

postgres=# CREATE USER "username" WITH ENCRYPTED PASSWORD 'password';

postgres=# GRANT ALL PRIVILEGES ON DATABASE databasename TO "username";

postgres=# \q
```

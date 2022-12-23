# Installation:
1. Mac Apple Silicon 
2. Ubuntu Linux 20.04

## Pyenv Setup on Mac Apple Silicon 

### 1. Install dependencies pyenv requires

```sh
brew update
brew install openssl readline sqlite3 xz zlib
```

### 2. If using Mojave or above, use this command to install an additional required dependency:

```sh
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```

### 3. Install pyenv

```sh
brew install pyenv
```

### 4. Update the shell to ensure autocomplete for pyenv is functional
```sh
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
exec "$SHELL"
```


## Pyenv Setup on Ubuntu Linux 20.04

### 1. Install all required prerequisite dependencies:

```sh
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

### 2. Download and execute installation script:

```sh
curl https://pyenv.run | bash
```

### 3. Add the following entries into your ~/.bashrc file:

```sh
# pyenv
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
```

### 4. Restart your shell:

```sh
exec $SHELL
```

### 5. Validate installation:

```sh
pyenv --version
```

# Uninstallation:

## 1. Remove the ~/.pyenv folder

```sh
Remove the ~/.pyenv folder
```

## 2. Delete the following entries from your ~/.bashrc file:

```sh
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
```

## 3. Restart your shell

```sh
exec $SHELL
```

# Basic commands:

## 1. To install specific version use install flag:

```sh
pyenv install 3.9.9
```

## 2. To list all available installed versions of Python on your system:

```sh
pyenv versions
```

## 3. To set a specific version of Python global (system-wide) use global flag:

```sh
pyenv global 3.9.9
```


## 4. To set a specific version of Python local (project-based) use local flag:

```sh
pyenv local 3.9.9
```

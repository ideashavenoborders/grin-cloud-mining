# Grin CPU Cloud Mining Guide - How to Mine GRIN in the Cloud

### 1. Create Server
First, create a new Ubuntu instance at your favorite cloud hosting provider. We use [DigitalOcean](https://m.do.co/c/8949111a23b1). Choose your size. In this guide we are using a CPU Optimized instances with 4 CPUs that currently cost $0.119/hour = $2.856 per 24 hours of mining. 

You can get about about 0.15-0.2 GPS per such instance.

In this guide, we will install both grin and grin-miner, configure grin-miner with your pool credentials, and start the mining process.

### 2. Log In and Build Grin
Connect to your server via SSH. 

#### Update Server

`apt update`

#### Install Dependencies

`apt-get install build-essential cmake git libgit2-dev clang libncurses5-dev libncursesw5-dev zlib1g-dev pkg-config libssl-dev`

#### Install Rust

`curl https://sh.rustup.rs -sSf | sh; source $HOME/.cargo/env`

#### Build Grin

`git clone https://github.com/mimblewimble/grin.git`

`cd grin`

The next step will take some time. Have a coffee, tea, or whatever kind of break that makes you grin.

`cargo build --release`

If all went well, you now have the grin binary in ~/grin/target/release.

To test, try to start it and have a look.

`cd target/release`

`./grin`

What you see is the Grin TUI (Text-User-Interface). You can quit the TUI for now by pressing Q on your keyboard.

#### Add Grin to Environment

`cd`

`nano .profile`

add the following line to the last line of your .profile file and save with *CTRL + O*:

`export PATH="$HOME/grin/target/release:$PATH"`

#### Prepare Configuration Files

`cd`

`source ~/.profile`

`cd grin`

`grin server config`


### 3. Prepare Grin Miner

`cd`

`git clone https://github.com/mimblewimble/grin-miner.git`

`cd grin-miner`

`git submodule update --init`

`cargo build`

#### Add grin-miner to Environment

`cd`

`nano .profile`

Add the following line to the last line of your .profile file and save:

`export PATH="$HOME/grin-miner/target/debug:$PATH"`


### Add Pool Credentials

Open grin-server.toml

`cd grin-miner`

`nano grin-server.toml`

You can use **CTRL + V** to jump forth one page, and **CTRL + Y** to jump back one page.

Jump to

```
#########################################
### MINING CLIENT CONFIGURATION       ###
######################################### 
```

Change the following 3 settings to your pool credentials, remove the comment tags:

``` 
# listening grin stratum server url
stratum_server_addr = "127.0.0.1:3416"

# login for the stratum server (if required)
#stratum_server_login = "http://192.168.1.100:3415"

# password for the stratum server (if required)
#stratum_server_password = "x" 
```

In our example, we use Grinmint. Check your Pool to find your personal credentials:

```
# listening grin stratum server url
stratum_server_addr = "eu-west-stratum.grinmint.com:3416"

# login for the stratum server (if required)
stratum_server_login = "mine+1242@mailhaven.com/rig1"

# password for the stratum server (if required)
stratum_server_password = "YourPassword"
``` 

*Please note: if you use more than 4 CPUs, you have to specify **nthreads** in your configuration file as well. If you follow this guide on a 4 CPU instance, you do not need to do this*

**CTRL + O** to save. 

Restart Ubuntu for all changes to take effect

`reboot`

Reconnect to your server via SSH.

### 4. Start Grin

To start grin, simply change to its dir and start it. Let's start it with screen, so that it keeps running.
`cd`

`screen -S grin`

`grin`

Congratulations! What you see is the Grin user interface. Play around a bit, if you like.

Press **CTRL+A+D** to detach from the screen view. If you want to reattach at any time, simply type 

`screen -rd grin`

and again deatach using **CTRL+A+D**

### 5. Start Grin-Miner

Just like the grin client, let's start grin-miner in screen to keep it running.

`cd ~/grin-miner`

`screen -S grin-miner`

`grin-miner`

Grin Miner will open. Check Connection Status. It will start mining right away. 

### 6. Enjoy

Nice one! You set up a Grin cloud mining op. 

Visit your pool's interface to check your mining statistics. Happy Grin mining!

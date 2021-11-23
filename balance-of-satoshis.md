---
layout: default
title: Balance of Satoshis
parent: Bonus Lightning
grand-parent: Bonus Section
nav_exclude: true
has_toc: false
---

# Balance of Satoshis
{: .no_toc }

Difficulty: Easy
{: .label .label-green }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Introduction

[Balance of Satoshis](https://github.com/alexbosworth/balanceofsatoshis) (BoS), created and maintained by LND developper Alex Bosworth, is a tool to work with LND channel balances.

*Requirements:*

* LND (or LND as part of Lightning Terminal)
* Requires an installation of Node v12.0+

*Acknowledgments:*

* This guide is modified from the [manual install guide](https://gist.github.com/openoms/823f99d1ab6e1d53285e489f7ba38602) for the Raspiblitz by @openoms.

## Check NodeJS

* NodeJS v12.0 or above should have been installed for the BTC RPC Explorer. We can check our version of NodeJS with user "admin": 

  ```sh
  $ node -v
  > v14.17.4
  ```

* If the version is v12.0 or above, you can move to section **Create bos user**.

* If NodeJS is not installed, follow [these commands](https://stadicus.github.io/RaspiBolt/raspibolt_55_explorer.html#install-nodejs) to install it.

* If NodeJS version is older than v12.0, you can attempt to upgrade it:
  * First, we need to clean the npm cache, install n (Node's version manager) and install the latest stable version
    
    ```sh
    $ sudo su
    $ npm cache clean -f
    $ npm install -g n
    $ sudo n stable
    $ exit
    $ node -v
    ```

## Create the bos user and set up npm-global

* Create the "bos" user, make "bos" a member of the "bitcoin" group and create a symlink to the LND directory

  ```sh
  $ sudo adduser --disabled-password --gecos "" bos
  $ sudo /usr/sbin/usermod --append --groups bitcoin bos
  $ sudo ln -s /mnt/ext/lnd/ /home/bos/.lnd
  $ sudo su - bos
  ```
  
* Set up nmp-global
 
  ```sh
  $ mkdir ~/.npm-global
  $ npm config set prefix '/home/bos/.npm-global'
  ```

* Update path (to avoid having to type the full path each time you'll invoke bos). First, open the .profile file with nano
  
  ```sh
  $ nano ~/.profile
  ```

* Add the following line at the end of the file, save and exit
 
  ```ini
  PATH="$HOME/.npm-global/bin:$PATH"
  ```

* Use the `source` command to make the change effective
 
  ```sh
  source ~/.profile
  ```
  
## Download source code and install bos
  
* Install bos

  ```sh
  $ npm install -g balanceofsatoshis --no-audit
  ```
  
## Using bos (with the "bos" user)

* To check the version, you can use one of the following command

  ```sh
  $ bos --version
  $ bos -V
  ```
  
* To see a list of available options and flags run
  
  ```sh
  $ bos help
  ```
  
* To see details about an individual command, add the name of the command, e.g. with the command `balance`
  
  ```sh
  $ bos help balance
  ```
A description of all the commands is also available here: [https://github.com/niteshbalusu11/BOS-Commands-Document](https://github.com/niteshbalusu11/BOS-Commands-Document) (note that this page might not be kept up-to-date)

* To rebalance on your node [A] a channel with high outbound (to node B) and a channel with high inbound (to node C)

  ```sh
  $ bos rebalance --amount [AMOUNT_IN_SATS] --max-fee-rate [TOTAL_MAX_FEE_RATE_OF_REBALANCING] --in [NODE_C_PUBKEY] --out [NODE_A_PUBKEY]
  ```
  Start with a small max-fee-rate and increase it if necessary.
  
* More information on bos commands [Github repository](https://github.com/alexbosworth/balanceofsatoshis).


## Upgrade

* Log in with the "bos" and upgrade Balance of Satoshis with `npm`

  ```sh
  $ sudo su - bos
  $ npm i -g balanceofsatoshis
  ```
  
## Uninstall

If you want to uninstall Balance of Satoshis:

* Log in with the "root" user and delete the "bos" user

  ```sh
  $ sudo su -
  $ userdel -r bos
  ``` 
  
## Optional: connect your node to a Telegram bot

* Balance of Satoshis allows connecting a node to a Telegram bot to receive updates about routing forwards, channel opening and closing events, successful rebalancing payments, payments received, keysend messages etc. It also saves a copy of `channel.backup` (SCB) each time there is a channel being opened or closed.
* Requirements: a Telegram account

### Create a new TG bot with the BotFather

* Open Telegram, in the general search box look for the [@BotFather](https://t.me/BotFather) bot and start a conversation with the bot.
* Type `/start`
* Type `/newbot` and follow the instructions (choose a bot name, username)
* Once the bot is created, the BotFather will give you a HTTP API token, copy it and keep it somewhere safe (like in a password manager). Note that if you lose this token, you could always get it agin by typing `/myBot` in the BotFather feed.
* You also get a link to your bot (in the form: t.me/[your_bot_username]) click on it and it will redirect you to your new bot feed. Keep Telegram open.

### Torify

To avoid leaking our node IP address to Telegram, we can force bos to use Tor using the [`torify`](https://gitlab.torproject.org/legacy/trac/-/wikis/doc/TorifyHOWTO) utility. To do this, we first need to edit the `torsocks.conf` file.

* Edit the `torsocks.conf` file by uncommenting the `AllowOutboundLocalhost` and setting the value to 1. Save (Ctrl+O) and exit (Ctrl+X)
  
  ```sh
  $ sudo nano /etc/tor/torsocks.conf
  ```
  ```ini
  AllowOutboundLocalhost 1
  ```

### Use bos to connect your node to the bot

* Open a SSH session with the `admin` user and change to the `bos` user
  
  ```sh
  $ sudo su - bos
  ```

* Now, we are going to request bos to connect our node to the TG bot
  
  ```sh
  $ /usr/bin/torify bos telegram
  ```
  
* When prompted, enter the HTTP API token that the @BotFather gave you earlier
* Go to your new TG bot feed and type `/connect`. Your bot will give you a connection code
* Copy the connection code and paste it in your SSH session in the second prompt that bos created. You should get a connection message on both your SSH session and your TG bot feed.
* Your TG bot will now receive notifications from your nodes for various events as described in the introduction.
* Leave the temporary session by pressing Ctrl+C

### Permannent connection and autostart on boot 

Now we’ll make sure our Telegram Bot command starts as a service on the Raspberry Pi so it’s always running.

* As user “admin”, create the service file.

  ```sh
  $ sudo nano /etc/systemd/system/bos-telegram.service
  ```
  
* Paste the following configuration. Replace <CONNECTION_CODE> with your own connection code. Save and exit.
  
  ```ini
  # /etc/systemd/system/bos-telegram.service

  [Unit]
  Description=bos-telegram
  Wants=lnd.service
  After=lnd.service

  [Service] 
  ExecStart=/usr/bin/torify /home/bos/.npm-global/bin/bos telegram --connect <CONNECTION_CODE>
  User=bos
  Restart=always
  TimeoutSec=120
  RestartSec=30
  StandardOutput=null
  StandardError=journal

  [Install]
  WantedBy=multi-user.target 
  ```
  
* Enable the service, start it and check the status of the service. You should also receive a connection message from your TG bot ('Connect to ...').
  
  ```sh
  $ sudo systemctl enable bos-telegram.service
  $ sudo systemctl start bos-telegram.service
  $ sudo systemctl status bos-telegram.service
  ``` 

---
layout: default
title: Bonus Section
nav_order: 60
has_children: false
has_toc: false
---
<!-- markdownlint-disable MD014 MD022 MD025 MD033 MD036 MD040 -->

# Bonus Section
{: .no_toc }

In this section, you can find various optional topics that make your RaspiBolt running even smoother. I split this up in various subsections, as the individual tasks can be quite long.

🚨 This section is not maintained as frequently and not tested as thoroughly as the main part of the guie. To use with caution!

*Difficulty:*
* 📗 Easy
* 📙 Intermediate
* 📕 Hard

## 1. Node maintenance
* **Dashboards & Appearance**
    * 📗 [System overview](bonus/raspibolt_61_system-overview.md) 
      >Your RaspiBolt will greet you with a quick system summary on login.
    * 📗 [Pimp the command line](bonus/raspibolt_62_commandline.md) 
      > Make your command line prompt shine with a golden ฿ and use more colors overall.
* **Privacy**
    * 📙 [Anonymous node with Tor](bonus/raspibolt_69_tor.md) 
      > Route all your Bitcoin traffic over the Tor network to stay anonymous and avoid leaking private information like your public IP address. Only for older configurations, the latest RaspiBolt v2 has this already enabled by default.
* **Resilience**
    * 📗 [System recovery](bonus/raspibolt_65_system-recovery.md)        
      > In case your microSD card gets corrupted or you brick your node, it's handy to have a quick recovery image at hand.
    * 📙 UPS (coming soon) 
      > Install a UPS and setup automatic garceful shutdown when battery is low. Prevents DB corruption during power issues.
* **Security**
    * 📗 [Hardware-based (Trezor) SSH authentification](bonus/raspibolt_75_trezor-agent.md) 
* **Hardware**
    * 📙 [Upgrade external drive](bonus/raspibolt_77_upgrade-external-drive.md) 
    * 📙 [Odroid setup: an alternative to Raspberry Pi](bonus/raspibolt_76_odroid_setup.md)
  

## 2. Bitcoin tools
* **Electrum Servers**
    * 📙 [Electrum Personal Server](bonus/raspibolt_64_electrum.md) 
      > The RaspiBolt is the perfect trustless Bitcoin backend for your regular on-chain transactions. Together with the Electrum wallet, it works even with your Ledger or Trezor hardware wallet. Mostly for older configurations, the latest RaspiBolt v2 already has a full Electrum server included by default.
* **Coinjoin (privacy)**
    * 📕 [JoinMarket on RaspiBolt](https://github.com/kristapsk/raspibolt-extras/blob/master/joinmarket.md) 
      > A CoinJoin software, which allows you to increase privacy and fungibility of on-chain Bitcoin transactions. It includes it's own Bitcoin wallet, backed by `bitcoind`, and uses market maker / market taker model, which means that either you pay small fee for having CoinJoin privacy fast (taker) or just keep software running and then you get paid for providing liquidity for CoinJoin's, in addition gaining privacy in a longer periods of time (maker). Even if you aren't interested in privacy of your coins, you can use JoinMarket for some little passive income from your bitcoins, without giving up your private keys. 
* **Multisig (security)**
    * 📙 [Specter Wallet](bonus/raspibolt_76_specter-desktop.md) 
      > The Specter Desktop app is a project that’s working on making a convenient and user-friendly GUI around Bitcoin Core with a focus on multisignature setup with hardware wallets and airgapped devices.

## 3. Lightning tools
* **Maintenance**
    * 📙 [Auto unlock LND on startup](bonus/raspibolt_6A_auto-unlock.md) 
      > Manually unlocking the LND wallet every time the system starts is not really feasible if your RaspiBolt is meant to run reliably somewhere in a closet. This script automatically unlocks the wallet on startup or service-restart. This comes at a minimal security cost, however, as the password needs to be stored on the device.
    * 📗 [Use lncli on a different computer](bonus/raspibolt_66_remote_lncli.md) 
       > Control your Lightning node from a different computer within you network, eg. from a Windows machine.
    * 📗 Circuit Breaker 
* **Operator dashboards & wallets**
  * *CLI-only*
    * 📗 [lntop](bonus/raspibolt_74_lntop.md) 
      > lntop is an interactive text-mode channels viewer for Unix systems.
    * 📗 [Additional scripts: show balance & channels](bonus/raspibolt_67_additional-scripts.md) 
      > These additional bash scripts display a balance overview (on-chain & in channels, active & inactive) as well as a nicely formatted channels overview.
  * *GUI - Web*
    * 📗 Ride the Lightning 
    * 📗 ThunderHub 
  * *GUI - Desktop*
    * 📗 [Zap](bonus/raspibolt_71_zap.md) 
      > The Zap desktop app (https://github.com/LN-Zap/zap-desktop) is a cross platform Lightning Network wallet focused on user experience and ease of use.
  * *GUI - Mobile*
    * 📙 [Zeus (Android)](bonus/zeus-over-tor.md) 
      >  Connect to your node over VPN or Tor, make payments with lightning or on-chain, manage your channels, and more. 
    * 📙 [Zap (Android)](bonus/raspibolt_72_zap-ios.md) 
      > The Zap Android app (https://zap.jackmallers.com) provides a neat interface for the RaspiBolt to manage peers & channels, make payments and create invoices.
 * **Liquidity management (rebalancing)**
    * 📙 Balance of Satoshis 
    * 📙 rebalance-lnd 
 * **Fee management**
    * 📙 charge-lnd 

## 4. Even more Extras
   * 🕮 [Extras by Rob Clark](https://github.com/robclark56/RaspiBolt-Extras/blob/master/README.md)
     *  Lights-Out: automatic unlocking of wallet and dynamic ip
     *  RaspiBoltDuo: testnet & mainnet running simultaneously
     *  Using REST access
     *  Receiving Lightning payments: automatically create invoices / qr codes
---

Next: [Troubleshooting](raspibolt_70_troubleshooting.md) >>

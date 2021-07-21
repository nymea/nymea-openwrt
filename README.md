# nymea-openwrt

OpenWrt feed for nymea installations.

This repository contains all related nymea packages for [OpenWrt](https://openwrt.org) systems. 

The feed provides all required packages (besides base feed) in order to build a nymea server for 
your OpenWrt system, including `qt5`.

## Installation

Add the package feed to your `feeds.conf`

    src-git nymea https://github.com/nymea/nymea-openwrt.git

Update the feefs you SDK or buildroot

    ./scripts/feeds update -a

Install the packages into your OpenWrt build system

    ./scripts/feeds install -a

You can enable to build nymea using `menuconfig` in the main menu under `nymea`-->`nymea`-->`nymead`

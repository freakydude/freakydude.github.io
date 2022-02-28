---
title: "Cheatsheet: Installing 'Oh My Fish'"
date: 2022-03-01
description: About installing a very helpful modern bash alternative in WSL2/Ubuntu
author: freakydude
draft: false
---

## Install Fish

1. Install fish
  `sudo apt install fish`
1. Switch to fish
  `fish`
1. Set fish as default shell
  `chsh -s /usr/bin/fish`

1. Switch back to bash
  `chsh -s /usr/bin/bash`

## Install 'Oh My Fish' with the theme 'Bob The Fish'

1. Install Oh My Fish
  `curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish`

1. Install Theme: Bob the Fish
  `omf install bobthefish`

## Install powerline fonts 

1. Install Powerline fonts (Ubuntu/Debian/Mint Linux)
  `sudo apt-get update`
  `sudo apt-get install fonts-powerline`

### Useful links

- [Shell: Fish](https://fishshell.com)
- [Project: Oh-my-fish](https://github.com/oh-my-fish/oh-my-fish)
- [Theme: Bob-the-fish](https://github.com/oh-my-fish/theme-bobthefish)
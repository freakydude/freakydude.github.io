---
title: "Set Up Rust and Bevy in WSL2 on Windows 11"
date: 2022-03-11
description: Some easy steps to set up an initial Rust development environment in Windows 11 WSL2/Ubuntu Linux, together with the libraries needed to compile and run all of the Bevy examples, using the brand-new preview of the graphical WSL implementation.
draft: false
tags:
  - rust
  - bevy
  - wsl
  - windows
  - linux
categories:
  - developer
---

Are you one of the nerds who like to develop your own game, fancy graphics, or any aspect that comes with that? Or would you like to take your first steps with the young programming language Rust, which unites the speed of C/C++ with memory and thread safety?

Why not combine both to have some fun learning new things?

Just to be even more nerdy, I show you a way to do all of that within the Windows Subsystem for Linux and its brand-new graphical extension under Windows 11. At the time of writing, it's still in preview, so be aware of some shortcomings.

## Requirements

- Microsoft Windows 11
- CPU virtualization enabled

## Install WSL2

If you haven't installed WSL2 yet, follow the [official WSL installation guide](https://learn.microsoft.com/en-us/windows/wsl/install).

In short:

1. Open a Windows terminal with administrator privileges ([Windows Terminal on GitHub](https://github.com/microsoft/terminal)).
1. Run

   ```bat
   wsl --install
   ```

   and follow the instructions.

## Configure Linux in WSL2

1. Open your Ubuntu Linux within WSL2.

1. Install Rust with

   ```sh
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   rustup update
   rustup component add rls rust-analysis rust-src
   ```

   _Remarks_: The Ubuntu 20.04 packages from the repository are missing libraries for code completion in VS Code.

   1. If you are using the fish shell:

      - Create or edit the file: `~/.config/fish/conf.d/myconfig.fish`
      - Add

        ```fish
        if status --is-login
          set -gx PATH $PATH ~/.cargo/bin $PATH
        end
        ```

1. Install the Bevy Linux dependencies with

   ```sh
   sudo apt install g++ pkg-config libx11-dev libasound2-dev libudev-dev libwayland-dev libxkbcommon-dev mesa-vulkan-drivers
   ```

   You can find more information in the [Bevy Linux dependencies documentation](https://github.com/bevyengine/bevy/blob/main/docs/linux_dependencies.md).

1. Install git

   ```sh
   sudo apt install git
   ```

## Try Bevy

Bevy is a slim and fast Rust game engine.

1. Clone the [Bevy repo](https://github.com/bevyengine/bevy):

   ```sh
   git clone https://github.com/bevyengine/bevy
   ```

2. Try the examples in the examples folder:

   ```sh
   cargo run --example breakout
   ```

Be aware that the graphical WSL for Linux GUI applications isn't hardware accelerated by default and is not free of errors at the time of writing. But hey, Microsoft is working hard on it. You can find more information in the [guide to Linux GUI apps on WSL](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps). With special drivers, you can already check that out, too.

## Conclusion

This is a very good starting point to learn Rust by playing around with a game engine. Let me know about your progress with it.

## Further links

- [Learn Rust](https://www.rust-lang.org/learn)
- [Bevy Engine](https://bevy.org/)
- [Bevy Getting Started](https://bevy.org/learn/quick-start/getting-started/)
- [Bevy Cheat Book](https://bevy-cheatbook.github.io/)
- [Linux GUI Apps on WSL](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps)

{{< support >}}

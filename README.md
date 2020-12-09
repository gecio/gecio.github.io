# Optimist Documentation

This repo primarily contains the documentation for the optimist platform.

## Getting started with the (experimental) docker environment

### Requirements:
- Visual Studio Code
- The [Remote Development Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
- Docker, like [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Procedure:
- Clone this repo
- Open it in Visual Studio Code
- VSCode will prompt, if you want to run the integrated dev container, select yes
- Wait for the build to finish, it might take some minutes on the first run
- Have fun.

### Hints:
- Your ssh agent and git configuration should automatically be available inside the dev container.
- You can change the devcontainer image to your preferences, by adding a [Dotfile-Repository](https://code.visualstudio.com/docs/remote/containers#_personalizing-with-dotfile-repositories)

## Running locally
Simple as that: `jekyll serve`
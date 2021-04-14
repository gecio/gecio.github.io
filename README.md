<br>

<div align="center">

# GEC Customer Documentation

This repository contains the customer documentation for GEC.

</div>

<br><br>

## Setup

Hint: A quickstart devcontainer is [down below](#Using-the-(experimental)-docker-environment).

To be able to build and run this project, Ruby 2.4 or higher is required. In addition, RubyGems, GCC and Make must be installed. See
**[Jekyll Installation](https://jekyllrb.com/docs/installation/)** for further details.

One those prerequisites are set up, install all dependencies by running:

```bash
bundle install
```

Now, the following commands are available:

| Command        | Description                |
| -------------- | -------------------------- |
| `jekyll serve` | Starts a development build |
| `jekyll build` | Creates a production build |

> Jekyll will put the build output in the `_site` folder, while also keeping a cache in `.jekyll-cache`

<br><br>

## Project

This project is structured the following way:

- Dependencies (e.g. theme, plugins) are defined in `Gemfile` and pinned by `Gemfile.lock`
- The Jekyll configuration (including configurations for theming and plugins) is defined in `_config.yml`
- Theme customizations (for the "Just the Docs" theme) are placed in
  - the `_includes` folder for custom HTML templates
  - the `_sass` folder for custom SASS styling

All other folders containing markdown files and assets are the actual site content. Sites that exist in multiple languages can be placed
right next to each other (e.g. `index.de.md` and `index.en.md`), and assets (such as images) can be placed right next to the pages using
them.

<br><br>

## Tools & dependencies

The following tools & dependencies are being used:

| Tool / dependencie                                                  | Description                                                  |
| ------------------------------------------------------------------- | ------------------------------------------------------------ |
| **[Jekyll](https://jekyllrb.com/)**                                 | Static Site Generator                                        |
| **[Just the Docs](https://github.com/pmarsceill/just-the-docs)**    | Jekyll theme                                                 |
| **[polyglot](https://github.com/untra/polyglot)**                   | Jekyll plugin for internationalization                       |
| **[jekyll-postfiles](https://github.com/nhoizey/jekyll-postfiles)** | Jekyll plugin for using assets placed next to markdown files |

<br><br>

## Using the (experimental) docker environment

### Requirements

- Visual Studio Code
- The [Remote Development Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
- Docker, like [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Procedure

- Clone this repository
- Open it in Visual Studio Code
- VSCode will prompt, if you want to run the integrated dev container, select yes
- Wait for the build to finish, it might take some minutes on the first run
- Have fun!

### Hints

- Your ssh agent and git configuration should automatically be available inside the dev container.
- You can change the devcontainer image to your preferences, by adding a [Dotfile-Repository](https://code.visualstudio.com/docs/remote/containers#_personalizing-with-dotfile-repositories)

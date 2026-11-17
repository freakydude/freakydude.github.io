# Published

You can find that site under

[https://blog.freakydu.de](https://blog.freakydu.de)

## Clone source

The theme [PaperMod](https://github.com/adityatelange/hugo-PaperMod) is a Git submodule, so clone with submodules:

```sh
git clone --recurse-submodules https://github.com/freakydude/freakydude.github.io
```

In an existing clone without the theme:

```sh
git submodule update --init --recursive
```

## Build and run hugo server

### With installed hugo packages

  ```sh
  hugo server --buildDrafts --disableFastRender
  ```

### With docker-compose

- Run "hugo server"

  ```sh
  docker compose up
  ```

- Build site

  ```sh
  docker compose run --rm hugo ""
  ```

- Build site with flags

  ```sh
  docker compose run --rm hugo --gc --minify --cleanDestinationDir
  ```

- Run a command of Hugo

  ```sh
  docker compose run --rm hugo env
  ```

# Published

You can find that site under

[https://blog.freakydu.de](https://blog.freakydu.de)

## Clone, build and run source

### Clone Source

```sh
git clone https://github.com/freakydude/freakydude.github.io
```

### Init ZZO submodule
```sh
git clone https://github.com/zzossig/hugo-theme-zzo.git themes/zzo
git submodule update --remote --merge
```

### Build and run hugo server

- With installed hugo packages
  ```sh
  hugo serve
  ```

- With docker-compose
  ```sh
  docker-compose up server
  ```
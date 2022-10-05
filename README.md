# Published

You can find that site under

[https://blog.freakydu.de](https://blog.freakydu.de)

## Clone, build and run source

### Clone Source

```sh
git clone https://github.com/freakydude/freakydude.github.io
```

### Init PaperMod submodule
```sh
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod  
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
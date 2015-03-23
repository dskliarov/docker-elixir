[azukiapp/elixir](http://images.azk.io/#/elixir)
==================

Base docker image to run **Elixir** applications in [`azk`](http://azk.io)

Versions (tags)
---

- [`latest`, `1`, `1.0`, `1.0.3`](https://github.com/azukiapp/docker-elixir/blob/master/Dockerfile)

Image content:
---

- Ubuntu 14.04
- Git
- VIM

Database:

- Erlang
- Elixir

### Usage with `azk`

Example of using this image with [azk](http://azk.io):

```js
/**
 * Documentation: http://docs.azk.io/Azkfile.js
 */
 
// Adds the systems that shape your system
systems({
  "my-app": {
    // Dependent systems
    depends: [], // postgres, mysql, mongodb ...
    // More images:  http://images.azk.io
    image: {"docker": "azukiapp/elixir"},
    // Steps to execute before running instances
    provision: [
      "mix do deps.get, compile"
    ],
    workdir: "/azk/#{manifest.dir}",
    shell: "/bin/bash",
    command: "mix run --no-halt",
    wait: {"retry": 20, "timeout": 1000},
    mounts: {
      '/azk/#{manifest.dir}': path("."),
    },
    scalable: {"default": 2},
    http: {
      domains: [ "#{system.name}.#{azk.default_domain}" ]
    },
    ports: {
      http: "4000",
    },
    envs: {
      // set instances variables
      HEX_HOME: "/azk/#{manifest.dir}/.hex/"
    },
  },
});
```

### Usage with `docker`

To create the image `azukiapp/elixir`, execute the following command on the elixir folder:

```sh
$ docker build -t azukiapp/elixir .
```

To run the image and bind to port 4000:

```sh
$ docker run -it --rm --name my-app -p 4000:4000 -v "$PWD":/myapp -w /myapp azukiapp/elixir mix run --no-halt
```

Logs
---

```sh
# with azk
$ azk logs my-app

# with docker
$ docker logs <CONTAINER_ID>
```

## License

Azuki Dockerfiles distributed under the [Apache License](https://github.com/azukiapp/dockerfiles/blob/master/LICENSE).
# Command line

On this page you'll find instructions on how to use the Spectre import tool's command line.

This page assumes you're self-hosting the Spectre import tool, although these commands also work when using Docker.

## Importing from Spectre

Use the following command to import from Spectre:

```bash
# self hosted
php artisan spectre:import config.json

# docker
docker exec -it <container> php artisan spectre:import config.json
```

JSON file `config.json` should point (obviously) to your JSON file. You can get one by importing once through the user interface first.

When you're using Docker, and you're not using the [instant import commands](../install/docker.md) make sure that you've mounted a directory with your files in it. This is something you must have done when you launched the Docker container, using for example `-v /path/to/my/files:/import`.

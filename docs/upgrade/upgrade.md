# Upgrade

## Upgrading a self-hosted Spectre / Salt Edge Importer

The best way to upgrade is to "reinstall" the Firefly III Spectre / Salt Edge importer using the following command:

```bash
composer create-project firefly-iii/spectre-importer --no-dev --prefer-dist updated-spectre-importer <next_version>
```

Where `<next_version>` is **[the latest version](https://version.firefly-iii.org/)** of the Firefly III Spectre / Salt Edge importer. This installs the tool in a new directory called `updated-spectre-importer`. 

Move over your `.env` file by copy-pasting it.

## Upgrading a Docker container

To upgrade, stop (if necessary) and remove your container using these commands:

```bash
docker stop <container>
docker rm <container>
```

To find out which container is the Firefly III Spectre / Salt Edge importer, run `docker container ls -a` and look for `fireflyiii/spectre-importer`.

Then pull the new image using this command:

```bash
docker pull fireflyiii/spectre-importer:latest
```

Create it again by running the command from the installation guide.

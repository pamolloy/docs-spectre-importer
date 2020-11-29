# Docker

The easiest way to use the Spectre importer is by using the Docker image.

There are some tricks you need to apply, when it comes to Docker and IP addresses, so please check out the instructions at the bottom of the page.

## Run as a web server

This is the easiest way to run the Spectre importer. Simply use the following run command to launch the Spectre importer.

```bash
docker run \
--rm \
-e FIREFLY_III_ACCESS_TOKEN= \
-e FIREFLY_III_URL= \
-e SPECTRE_APP_ID= \
-e SPECTRE_SECRET= \
-p 8081:8080 \
fireflyiii/spectre-importer:latest

```

By running this script, you will start a web server on port 8081 that will allow you to import data. You should append the command with your Personal Access Token, Firefly III URL and the Spectre API App ID and secret. You can get these on the [Spectre website](https://www.saltedge.com/clients/profile/secrets). If necessary you create a new key using the "New API key" button. Select "Service" as the type.

Here are some tricks to make it easier for yourself:

### Use pre-defined script

Use [run-hosted.sh](https://raw.githubusercontent.com/firefly-iii/spectre-importer-docker/main/run-hosted.sh) to make it easier to manage your Personal Access Token and Spectre / Salt Edge App ID and secret.

### Append "-d"

Change `docker run` to `docker run -d` to make sure the image runs in the background.

## Run inline

This is an alternative option.

This command will launch the Spectre importer. It will try to import everything from Spectre based on the configuration file in `/home/james/spectre_config_file.json`. This is fully automated. Check it out:

```bash
docker run \
--rm \
-v /home/james/spectre_config_file.json:/import/spectre.json \
-e FIREFLY_III_ACCESS_TOKEN= \
-e FIREFLY_III_URL= \
-e SPECTRE_APP_ID= \
-e SPECTRE_SECRET= \
-e WEB_SERVER=false \
fireflyiii/spectre-importer:latest
```

The advantage of this piece of code is that with a working configuration file, you can automate the import. If course, your configuration file may be stored in another location, so change it as necessary. Here too, you need a working Firefly III API key and a Spectre App ID and secret.

This can also be made easier for yourself:

### Use pre-defined script

Use [run-inline.sh](https://github.com/firefly-iii/spectre-importer-docker/blob/main/run-inline.sh) to make it easier to manage your Personal Access Token and Spectre access data.

## Docker and IP addresses

If you run the Spectre importer through Docker the IP address you need to contact Firefly III is **not** `127.0.0.1`, even when you run Firefly III on the same machine. Docker uses an internal network. There's a good chance your Firefly III installation has an IP address that starts with `172.17`. You can find out the internal IP address of Firefly III using this command:

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' CONTAINER
```

Instead of `CONTAINER`, use the container ID of your Firefly III container.

If your Firefly III installation is online, you can also use the web address. If you want to, you can generate a Personal Access Token on the [demo site](https://demo.firefly-iii.org/) and use the demo site as a test. Keep in mind that the demo site is **public** to everybody so everyone will see what you import.

**Remember**, by default Firefly III runs on port 8080.

### Example scripts for a full setup

The commands below set up a basic MariaDB instance and an installation of Firefly III. It will then start the Spectre importer. This is just to show you what the relationship is between these different Docker images.

```bash
# run a basic MariaDB instance.
docker run --name mariadb -e MYSQL_ROOT_PASSWORD=super_secret -e MYSQL_DATABASE=fireflyiii -d mariadb:latest

# get the IP of the MariaDB instance:
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mariadb

# run a basic Firefly III instance (update the IP of the database if necessary)
docker run -d \
--name fireflyiii \
-v firefly_iii_export:/var/www/firefly-iii/storage/export \
-v firefly_iii_upload:/var/www/firefly-iii/storage/upload \
-p 80:8080 \
-e APP_KEY=123456789012345678901234567890aa \
-e DB_HOST=172.17.0.2 \
-e DB_CONNECTION=mysql \
-e DB_PORT=3306 \
-e DB_DATABASE=fireflyiii \
-e DB_USERNAME=root \
-e DB_PASSWORD=super_secret \
jc5x/firefly-iii:latest

# Chase the log files to see when Firefly III is ready to go:
docker logs -f $(docker container ls -a -f name=fireflyiii --format="{{.ID}}")

# get Firefly III IP address:
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker container ls -a -f name=fireflyiii --format="{{.ID}}")

# Adapt run-inline.sh and run it using your personal access token and Spectre / Salt Edge App ID and secret
# You can find it here: https://github.com/firefly-iii/spectre-importer-docker/blob/main/run-inline.sh

./run-inline.sh

```

**Remember**, by default Firefly III runs on port 8080.

# Configure install

There are four pieces of configuration required for the Firefly III Spectre / Salt Edge import tool:

- A Personal Access Token
- The URL / IP address of your Firefly III instance
- Your Spectre / Salt Edge App Secret (select "Service" as the type)
- Your Spectre / Salt Edge App ID

## Configuration location

The Firefly III URL and the Personal Access Token are stored in two environment variables, `FIREFLY_III_URI` and `FIREFLY_III_ACCESS_TOKEN` respectively. You can use the `.env` file to store them, use Docker's `-e` flag to set them or use your operating system to set these values. Check out the installation page for more information.

## Personal Access Token

You can generate your own Personal Access Token on the Profile page. Login to your Firefly III instance, go to "Options" > "Profile" and find the "Personal Access Tokens" at the bottom of the page. Create a new Personal Access Token by clicking on "Create New Token". Give it a recognizable name and press "Create".

The Personal Access Token is pretty long. Use a tool like Notepad++ or Visual Studio Code to copy-and-paste it.

## URL or IP

You need to know the IP address or website address of your own Firefly III instances. In many cases this is simply `http://localhost`, but if you're a fancy user it might be something like `https://finances.example.com` or something similar.

## Spectre access data

You can generate an app secret and ID on the [Salt Edge website](https://www.saltedge.com/clients/profile/secrets). Make sure you generate a **service key**, not an app key.

### Localhost and Docker? Be careful!

If you host Firefly III on `http://localhost` and you're using Docker, beware. The Spectre / Salt Edge importer may *not* be able to contact Firefly III. From the perspective of the Spectre / Salt Edge importer, "localhost" is referring to the Spectre / Salt Edge importer itself, not to Firefly III. 

Make sure you use the internal IP address of your Firefly III Docker container. You can get this IP address by using the following command:

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' CONTAINER
```

Instead of `CONTAINER`, use the container ID of your Firefly III installation.

**Remember**, by default Firefly III runs on port 8080.

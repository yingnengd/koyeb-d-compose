
<a href="https://app.koyeb.com/deploy?name=docker-compose-on-koyeb&amp;privileged=true&amp;type=git&amp;repository=github.com/yingnengd/koyeb-d-compose&amp;branch=main&amp;builder=dockerfile&amp;dockerfile=Dockerfile.koyeb" rel="nofollow"><img src="https://camo.githubusercontent.com/8f91e42463870497e0309c48e99d9e455d62f8d454c809e78b154384dd24329d/68747470733a2f2f7777772e6b6f7965622e636f6d2f7374617469632f696d616765732f6465706c6f792f627574746f6e2e737667" alt="Deploy to Koyeb" data-canonical-src="https://www.koyeb.com/static/images/deploy/button.svg" style="max-width: 100%;"></a>

## Usage

Let's say you have a GitHub repository containing a `docker-compose.yml` file that you want to deploy on Koyeb. Your application could be the example application from the [docker-compose documentation](https://docs.docker.com/compose/gettingstarted/).

First, add a new file called `Dockerfile.koyeb` with the following content in your repository:

```dockerfile
FROM koyeb/docker-compose

COPY . /app
```

The image expects the `/app` directory to contain the [Compose file](https://docs.docker.com/compose/compose-file/03-compose-file/) and all of the other files required to run your project.

### Deploy with the control panel

To deploy your service using the [control panel](https://app.koyeb.com/), follow these steps:

* Create a new Service on Koyeb.
* Select the "GitHub" deployment method.
* Select your repository and choose **Dockerfile** as your builder.
* In the "build and deployment settings", set the **Dockerfile location** to `Dockerfile.koyeb` and enable the **Privileged** flag.
* In the "Exposing your service" section, add the port your application listens on. This is important because otherwise, the application will not become healthy and traffic will not be routed to it.
* Click **Deploy** to create the Service!


### Deploy with the CLI

To deploy your service using the [Koyeb CLI](https://github.com/koyeb/koyeb-cli), follow these steps:

Create a new application:

```bash
$ koyeb app create my-compose-app
```

Create a new service:

```bash
$ koyeb service create my-compose-service \
    --app my-compose-app \
    --git github.com/<OWNER>/<REPO> \
    --git-branch master \
    --git-builder docker \
    --git-docker-dockerfile Dockerfile.koyeb \
    --privileged \
    --port 8000 \
    --route /:8000
```

* Replace `<OWNER>` and `<REPO>` with the owner and the name of your GitHub repository.
* Set `--port` and `--route` according to your application.

# vue-in-docker- [vue-in-docker](#vue-in-docker)
- [vue-in-docker- vue-in-docker](#vue-in-docker--vue-in-docker)
  - [Using Docker to create a new Vue.js app](#using-docker-to-create-a-new-vuejs-app)
  - [Setting up the Dockerfile and the environment](#setting-up-the-dockerfile-and-the-environment)


Following [This tutorial](https://v2.vuejs.org/v2/cookbook/dockerize-vuejs-app)

## Using Docker to create a new Vue.js app

First run:

```bash
docker run --rm -v ${PWD}:/app -w /app node:lts-alpine npx @vue/cli create my-vue-app --default
```
If necessary, add permissions to your user to the contents of my-vue-app (as they were created by the container)

```bash
sudo chown -R $USER:$USER my-vue-app
```

## Setting up the Dockerfile and the environment

Create the Dockerfile (in the my-vue-app folder) and then

Building the container:

```bash
docker build -t my-vuejs-app .
```

```bash
docker run -it -p 8080:8080 --rm --name my-vuejs-app-container my-vuejs-app
```


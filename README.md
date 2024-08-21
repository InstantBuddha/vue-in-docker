# vue-in-docker- [vue-in-docker](#vue-in-docker)
- [vue-in-docker- vue-in-docker](#vue-in-docker--vue-in-docker)
  - [Using Docker to create a new Vue.js app](#using-docker-to-create-a-new-vuejs-app)
  - [Setting up the Dockerfile and the environment](#setting-up-the-dockerfile-and-the-environment)
  - [Development version](#development-version)
  - [Update dependencies](#update-dependencies)
  - [Modify .eslintrc.js](#modify-eslintrcjs)
  - [Vue notes](#vue-notes)


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

## Development version

Use Dockerfile.dev

```bash
# build it first
docker build -f Dockerfile.dev -t my-vuejs-app-dev .

# and then:
docker run -it -p 8080:8080 -v ${PWD}:/app --rm --name my-vuejs-app-dev-container my-vuejs-app-dev
```

## Update dependencies

```bash
# Sh in first:
docker exec -it my-vuejs-app-dev-container sh

# List outdated dependencies:
npm outdated

# Update dependencies:
npm update

# After updating rebuilding the Vue app might be necessary:
npm run build

```

To install plugins to an already created project [link](https://cli.vuejs.org/guide/plugins-and-presets.html#installing-plugins-in-an-existing-project)

## Modify .eslintrc.js

It is necessary for the linter to recognise certain things such as macros. See in the updated files.

## Vue notes

[Read about Vue basics](01-Vue-basics.md)
[Read information on Vue components](02-component-information.md)
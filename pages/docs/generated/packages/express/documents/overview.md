[Express](https://expressjs.com/) is mature, minimal, and an open source web framework for making web applications and
apis.

## Setting Up Express

To create a new workspace with Express, run the following command:

```shell
 npx create-nx-workspace --preset=express
```

### Adding Express to an Existing Project

Install the express plugin

{% tabs %}
{%tab label="npm"%}

```shell
npm i --save-dev @nx/express
```

{% /tab %}
{%tab label="yarn"%}

```shell
yarn add --dev @nx/express
```

{% /tab %}
{% /tabs %}

## Creating Applications

Add a new application to your workspace with the following command:

```shell
nx g @nx/express:app my-app
```

Serve the application by running

```shell
nx serve my-app
```

This starts the application on localhost:3333/api by default.

> Express does not come with any library generators, but you can leverage the[`@nx/js`](/packages/js#create-libraries) plugin to generate a Node.js library for your express application.

### Application Proxies

The Express application generator has an option to configure other projects in the workspace to proxy API requests. This
can be done by passing the `--frontendProject` with the project name you wish to enable proxy support for.

```shell
nx g @nx/express:app <express-app> --frontendProject my-react-app
```

## Using Express

### Testing Projects

You can run unit tests with:

```shell
nx test <project-name>
```

### Building Projects

Express projects can be built with:

```shell
nx build <project-name>
```

Build artifacts will be found in the `dist` directory under `apps/<project-name>` by default. Customize the build
configuration by editing `outputPath` in the [project configuration](/reference/project-configuration).

### Waiting for Other Tasks

You can wait for other tasks to run before serving the express app which can be handy for spinning up various services
the application depends on— for example, other apis in a microservice.

Setting the `waitUntilTargets` option with an array of targets (format: `"project:target"`) executes those tasks
before serving the Express application.

## More Documentation

- [Using Jest](/packages/jest)
- [@nx/js](/packages/js)
- [Express](https://expressjs.com/)

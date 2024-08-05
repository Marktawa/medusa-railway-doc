# Build and Deploy Medusa: Using Railway, Cloudflare, and NeonDB

<!--Article for Deploy Medusa on Railway-->

In this article you will learn how to build deploy a Medusa store. You will deploy your Medusa backend server to Railway, the Store Admin frontend to Cloudflare and the Storefront to Cloudflare as well. You will use Neon to host your PostgreSQL database and GitHub to store the source code for production.

The method used here is a manual method. Hopefully, in the near future I will provide an article on an automated method of deployment.

Before starting you must have the following:

- Node.js (version 16.14.0 or higher) - [Download it here](https://nodejs.org/en/download/package-manager)
- Git - [Download it here](https://git-scm.com/downloads)
- Railway account - [Sign up here](https://railway.app/login)
- GitHub account - [Sign up here](https://github.com/join)
- Cloudflare account - [Sign up here](https://dash.cloudflare.com/sign-up/)
- Neon account - [Sign up here](https://console.neon.tech/signup)

Before proceeding with the tutorial you can check out the following links for useful resources:

<!-- - [Video demo]() -->
<!-- - [Live link of the app]() -->
- [GitHub repo](https://github.com/Marktawa/medusa-railway)

## Install Medusa Server locally

In this step you will install the Medusa backend server on your local machine.

[Create a new GitHub repo](https://repo.new) and give it a suitable name. For example, `medusa-store`. 

Open up your terminal and clone it to your local machine. 

```sh
git clone https://<your-github-username>/medusa-store.git
```

A new directory named `medusa-store` is created. This repo will contain all the source files for your Medusa store project.

```sh
mkdir medusa-starter
```

### Create PostgreSQL Database for Medusa store on Neon

Visit [Neon - Sign Up](https://console.neon.tech/signup) page if you haven't signed up for a Neon account already.

![Neon Sign Up Page](https://res.cloudinary.com/craigsims808/image/upload/v1722858546/articles/railway-medusa/neon-sign-up_hvem5d.png)

[Create a new project](https://console.neon.tech/app/projects) in the Neon console. Give your project a name like `medusastore` and your database a name like `storedb` then click on **Create project**.

![New Neon Project](https://res.cloudinary.com/craigsims808/image/upload/v1722858546/articles/railway-medusa/neon-sign-up_hvem5d.png)

Copy your connection string which will look something like: `postgres://dominggobana:JyyuEdr809p@df-hidden-bonus-ertd7sio.us-east-3.aws.neon.tech/storedb?sslmode=require`. It is in the form `postgres://[user]:[password]@[host]/[dbname]. You will provide this connection string as a database URL to your Medusa server in the next steps.

### Install Medusa CLI

Open up the `medusa-store` folder in your terminal and run the following command to install the Medusa CLI.

```sh
npm install @medusajs/medusa-cli -g
```

We will use the Medusa CLI to install the Medusa server.

### Create a new Medusa project

Run the following command in your terminal inside the `medusa-store` folder to create a new Medusa project, and name it `medusa-backend`.

```sh
medusa new medusa-backend
```

You will be asked to specify your PostgreSQL credentials. Choose "Skip database setup". Wait for the command to execute and then a new directory named `medusa-backend` will be created to store the server files.

### Configure PostgreSQL Database

Add the connection string to your Neon hosted PostgreSQL database you copied earlier as the `DATABASE_URL` to your environment variables. Inside `medusa-backend` create a `.env` file.

```sh
cd medusa-backend
touch .env
```

And add the following:

```
DATABASE_URL=postgresql://dominggobana:JyyuEdr809p@df-hidden-bonus-ertd7sio.us-east-3.aws.neon.tech/storedb?sslmode=require
```

### Seed Database

Run migrations and seed data to the database by running the following command inside your `medusa-backend` directory:

```sh
medusa seed --seed-file="./data/seed.json"
```

Wait for a while, as your database will be populated with dummy product data.

### Start your Medusa backend

After the seeding your database, running the following command to start your Medusa server.

```sh
npx medusa develop
```

The Medusa server will start running on port 9000. The Medusa Admin User Interface will run on port 7001.

Test your server by running the following command in a new terminal session:

```sh
curl localhost:9000/store/products
```

If the server is working correctly, you should see a list of products.

Visit [localhost:7001](http://localhost:7001) in your browser to see the Medusa Admin Dashboard. Use the Email `admin@medusa-test.com` and Password `supersecret` to log in.

![Medusa Admin Dashboard]()

In the **Products** section of your dashboard you should see a list of products. This is the dummy data you seeded into your database in the previous step.

## Install Medusa Storefront Locally

In this step, you will install the Medusa Next.js Storefront Starter.

Open up the `medusa-store` folder in a new terminal session. Create a new Next.js project using the [Medusa Starter Storefront](https://github.com/medusajs/nextjs-starter-medusa) by running the following command:

```sh
npx create-next-app -e https://github.com/medusajs/nextjs-starter-medusa medusa-storefront
```

Wait for the storefront to be installed then change to the newly created `medusa-storefront` directory. Inside `medusa-storefront` rename the template environment variable file to use environment variables in development:

```sh
cd medusa-storefront
mv .env.template .env.local
```

Make sure the Medusa backend is running, then run the local Next.js server:

```sh
npm run dev
```

Your Next.js Starter Storefront will start running on port `8000`. Visit [localhost:8000](http://localhost:8000) in your browser to see the storefront.

![Medusa Storefront Home Page](/tf.png)

If all is working you should see the storefront with products from your backend displayed.

## Test Deployment Locally

After confirming that the Medusa Backend server, Admin Interface and the Storefront are all working correctly, it is now time to test the deployment locally.

The Medusa Admin will be deployed separately from the Medusa Backend Server. This set up is supported by the Railway Free Plan.

<!--
#### Create a New Neon Database Branch for Testing

With Neon, you can quickly and cost-effectively branch your data for development, testing, and various other purposes, enabling you to improve developer productivity and optimize continuous integration and delivery (CI/CD) pipelines.

To create a branch for your Medusa store database, visit the Neon console. Make sure the project for your Medusa store is selected. In our case it's named `medusastore`. Select **Branches** and click **Create branch** to open the branch creation dialog:

![Create new branch in Neon console]()

Give your branch a name like `storedbtest`, since this database is just for testing purposes. The parent branch will be the default branch for your project which we named `storedb`.

Click **Create new branch** to create your branch. You will be redirected to the **Branch** overview page where you are shown the details for your new branch.

Copy the connection string for your test database.
-->

### Build and Test Medusa Backend Locally

<!--
Stop your Medusa server if it is running. 

Create a new branch in your repo named `server` and switch over to it.

```sh
git branch test
git switch
```
-->

Open up the `medusa-store` directory in a new terminal session. Create a new directory named `medusa-server`.

```sh
mkdir medusa-server
```

Copy the contents of the `medusa-backend` directory into the newly created `medusa-server` folder.

```sh
cp -r medusa-backend/ medusa-server/
```

The `medusa-server` directory will be used to deploy the Medusa server.

<!--
Update `.env` which contains your environment variables inside `medusa-server` for your test build. You will use it to configure the `PORT`, `JWT_SECRET`, `COOKIE_SECRET`, and `DATABASE_URL`. Add the values as shown below and paste the connection string to your test database branch as a value to the `DATABASE_URL`.

```
PORT=9000
JWT_SECRET=something
COOKIE_SECRET=something
DATABASE_URL=postgresql://dominggobana:JyyuEdr809p@df-hidden-bonus-ertd7sio.us-east-3.aws.neon.tech/storedb?sslmode=require
```

> NOTE: The values for `JWT_SECRET` and `COOKIE_SECRET` should be something unique and secure
-->

Since we are deploying the Medusa admin separately, disable the admin plugin's [serve option](https://docs.medusajs.com/admin/configuration#plugin-options).

Open up `medusa.config.js` inside `medusa-server` and update it as follows:

```js
const plugins = [
  // ...
  {
    resolve: "@medusajs/admin",
    /** @type {import('@medusajs/admin').PluginOptions} */
    options: {
      // only enable `serve` in development
      // you may need to add the NODE_ENV variable
      // manually
      serve: process.env.NODE_ENV === "development",
      // other options...
      autoRebuild: true,
      develop: {
        open: process.env.OPEN_BROWSER !== "false",
      },
    },
  },
]
```

This ensures that the admin isn't built or served in production. You can also change `@medusajs/admin` dependency to be a devdependency in `package.json`.

Also, change the `build` command to remove the command that builds the admin inside the `package.json` file:

```js
"scripts": {
  // ...
  "build": "cross-env npm run clean && npm run build:server",
}
//...
"devDependencies": {
  //...
  "@medusajs/admin": "^7.1.11",
}
```

Then run the following command inside the `medusa-server` directory to build the Medusa server:

```sh
npm run build
```

After the build process finishes run migrations using the following command:

```sh
npx medusa migrations run
```

Once migrations run and finish, start the Medusa server using the following command:

```sh
npx medusa start
```

Wait for a bit until the server starts running, then visit [localhost:9000/store/products](http://localhost:9000/store/products) in your browser. You should a json of products available on your backend.

![Medusa server local deploy test]()

Visit [localhost:9000/health](http://localhost:9000/health) in your browser to get the health status of your locally deployed backend.

![Medusa local test deployment health]()

### Build and Test Medusa Admin Locally

<!--
Open up `medusa-store` in a new terminal session. Switch to the main branch of the repo.

```sh
git switch main
```

From the `main` branch, create a new branch named `admin` and switch over to it.

```sh
git branch admin
git switch admin
```
-->

Even though you are deploying the admin separately, you must include the entire Medusa backend project in the deployed folder. The build process for the admin uses the backend server's project files.

Open up the `medusa-store` directory in a new terminal session. Create a new directory named `medusa-admin`.

```sh
mkdir medusa-admin
```

Copy the contents of the `medusa-backend` directory into the newly created `medusa-admin` folder.

```sh
cp -r medusa-backend/ medusa-admin/
```

The `medusa-admin` directory will be used to deploy the Medusa Admin Dashboard.

In the `package.json` file of your `medusa-admin` directory, add or change the build script for the admin:

```json
"scripts": {
    //other scripts
    "build:admin": "medusa-admin build --deployment",
}
```

<!--
Replace the contents of `.env` with the backend's URL stored as `MEDUSA_ADMIN_BACKEND_URL`:

```
MEDUSA_ADMIN_BACKEND_URL=http://localhost:9000
```
-->

Now run the following command to build the Medusa Admin:

```sh
npm run build:admin
```

Wait for a while for the project to build. 

After it finishes, start the admin server.

```sh
npm run start
```

Visit [localhost:7001](http://localhost:7001) in your browser to view the admin.

![Medusa Admin Deployed Locally]()

### Build Medusa and Test Storefront Locally

Open up the `medusa-store` directory in a new terminal session. Open the `medusa-storefront` directory (the directory containing your storefront's source code).

Build the Medusa storefront.

```sh
npm run build
```

When it finishes building, start the Medusa storefront server

```sh
npm run start
```

Visit [localhost:8000](http://localhost:8000) in your browser to view your storefront's deployment.

![Locally Deployed Storefront]()

## Deploy Storefront to Cloudflare

We will deploy the Medusa Starter Storefront to Cloudflare Pages.

### Create GitHub Repo

In your local machine, navigate to the folder, `medusa-storefront` containing your Medusa storefront starter source code and create a new GitHub repo based on this folder.

<!--
### Next.js Cloudflare Configuration

To use Next.js with Cloudflare Pages, you need to add the `@cloudflare/next-on-pages` CLI tool. 

Open up `medusa-storefront` folder in your terminal and add the `@cloudflare/next-on-pages`
-->

### Prepare Deployment on Cloudflare

Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com/) and select **Workers & Pages**.

![Pages in Cloudflare Dashboard](https://res.cloudinary.com/craigsims808/image/upload/v1720439036/articles/medusa-sveltekit/cloudflare-dashboard_q2rvwf.png)

Select **Create application** then **Pages** and **Connect to Git**.

You will be prompted to sign in with your preferred Git provider.

Next, select the GitHub project for your Medusa Storefront repo.

Once you have selected a repository, select **Install & Authorize** and **Begin setup**.

You can then customize your deployment in **Set up builds and deployments**.

### Configure Build Settings

Set the build command of your deployed project to use the `build` command:

```sh
npm run build
```

Set the output directory of your deployed project to `build`.

Copy the environment variables from `.env.local` to the **Environment Variables** section.

![Build settings - Storefront]()

### Save Configuration and Deploy Storefront

After you have finished setting your build configuration, select **Save and Deploy**. Your project build logs will output as Cloudflare Pages installs your project dependencies, builds the project, and deploys it to Cloudflare’s global network.

When your project has finished deploying, you will receive a unique URL to view your deployed site. This unique URL will be used as the value for the `STORE_CORS` environment variable when you deploy the Medusa backend server.

![Cloudflare Successful Deploy]()

## Deploy Medusa Admin to Cloudflare

We will deploy the Medusa Admin application on Cloudflare Pages.

### Create GitHub Repo

Hosting providers like Cloudflare allow you to deploy your project directly from GitHub. This makes it easier for you to push changes and updates without having to manually trigger the update in the hosting provider.

> **NOTE:**
>
>*Even though you are just deploying the admin, you must include the entire Medusa backend project in the deployed repository. The build process of the admin uses many of the backend's repos.*

Navigate to the Medusa server directory `my-medusa-store` of your project folder in your local machine. Duplicate the directory and create a new GitHub repo based on this directory to handle all the config related to the admin only.

### Configure Build Command

In the `package.json` file of your new Medusa Admin repo, add or change the build script for the admin:

```json
"scripts": {
  //other scripts
  "build:admin": "medusa-admin build --deployment",
}
```

> **NOTE:**
>
> When using `--deployment` option, the backend's URL is loaded from the `MEDUSA_ADMIN_BACKEND_URL` environment variable. You will configure this environment variable in a later step.

### Preparing Deployment

Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com/) and select **Workers & Pages**.

![Cloudflare Dashboard](https://res.cloudinary.com/craigsims808/image/upload/v1720439036/articles/medusa-sveltekit/cloudflare-dashboard_q2rvwf.png)

Select **Create application** then **Pages** then **Connect to Git**.

You will be prompted to sign in with your preferred Git provider.

Next, select the GitHub project for your Medusa Admin repo.

Once you have selected a repository, select **Install & Authorize** and **Begin setup**. 

![Select GitHub repo](https://res.cloudinary.com/craigsims808/image/upload/v1720439067/articles/medusa-sveltekit/select-github-repo-cloudflare_ea42tu.png)

You can then customize your deployment in **Set up builds and deployments**.

Your **project name** will be used to generate your project's hostname.

### Configure Build settings

Set the build command of your deployed project to use the `build:admin` command:

```bash
npm run build:admin
```

Set the output directory of your deployed project to `build`.

Add the environment variable `MEDUSA_ADMIN_BACKEND_URL` and set its value to `localhost:9000` provisionally.

![Cloudflare Set up builds and deployments](https://res.cloudinary.com/craigsims808/image/upload/v1720439037/articles/medusa-sveltekit/build-settings-cloudflare_lfqsnt.png)

### Save Configuration and Deploy Admin

After you have finished setting your build configuration, select **Save and Deploy**. Your project build logs will output as Cloudflare Pages installs your project dependencies, builds the project, and deploys it to Cloudflare’s global network.

When your project has finished deploying, you will receive a unique URL to view your deployed site. This unique URL will be used as the value for the `ADMIN_CORS` environment variable when you deploy the Medusa backend server.

![Cloudflare Page Admin URL](https://res.cloudinary.com/craigsims808/image/upload/v1720439037/articles/medusa-sveltekit/cloudflare-admin-deploy-success_sy3uum.png)

## Deploy Medusa Server to Railway

We will deploy the Medusa Backend Server on [Railway](https://railway.app/). Railway provides a free trial (no credit card required) that allows you to deploy your Medusa backend along with PostgreSQL and Redis databases. This is useful mainly for development and demo purposes. [Sign up for a Railway account](https://railway.app/login) if you haven't already and proceed with the following steps.

### Create GitHub repo

Navigate to your Medusa store directory `medusa-store` in your local machine. Create a new GitHub repo using the `medusa-server` directory. This will be the GitHub repo that Railway will use for deploying the Medusa backend server for your store.

### Add Railway Configuration File

Add in the root of `medusa-server` the file, `railway.toml`, with the content based on the package manager of your choice:

> **NOTE*:
>
> To avoid errors during the installation process, it's recommended to use `yarn` for installing the dependencies. Alternatively, pass the `--legacy-peer-deps` option to the `npm` command.

Using `yarn`:

```toml
[build]
builder = "NIXPACKS"

[build.nixpacksPlan.phases.setup]
nixPkgs = ["nodejs", "yarn"]

[build.nixpacksPlan.phases.install]
cmds=["yarn install"]
```

Or using `npm`:

```toml
[build]
builder = "NIXPACKS"

[build.nixpacksPlan.phases.setup]
nixPkgs = ["nodejs", "npm"]

[build.nixpacksPlan.phases.install]
cmds=["npm install --legacy-peer-deps"]
```

### Configure Server for Production

Open the `medusa-config.js` file in your new server repo.

Update the following parts to enable caching using Redis.

Uncomment the inner part of the following section:

```js
const modules = {
  /*eventBus:
    resolve: "@medusajs/event-bus-redis",
```

Uncomment the following section as well:

```js
// Uncomment the following lines to enable REDIS
// redis_url: REDIS_URL
```

It then becomes:

```js
//...
const modules = {
  eventBus: {
    resolve: "@medusajs/event-bus-redis",
    options: {
      redisUrl: REDIS_URL
    }
  },
  cacheService: {
    resolve: "@medusajs/cache-redis",
    options: {
      redisUrl: REDIS_URL
    }
  },
};

/** @type {import('@medusajs/medusa').ConfigModule["projectConfig"]} */
const projectConfig = {
  jwtSecret: process.env.JWT_SECRET,
  cookieSecret: process.env.COOKIE_SECRET,
  store_cors: STORE_CORS,
  database_url: DATABASE_URL,
  admin_cors: ADMIN_CORS,
  redis_url: REDIS_URL
};
//...
```

Commit your changes, and push them to your remote GitHub repository. Once your repository is ready on GitHub, log in to your Railway dashboard.

### Create Project + Redis

On the Railway Dashboard, click on the **New Project** button and choose from the list the **Deploy Redis** option.

![New Railway Project]()

A Redis instance will be created and, after a few seconds, you will be redirected to your project page.

![Railway Project Dashboard]()

### Deploy Medusa in Server Mode

In this section, you'll create a Medusa backend instance running in `server` runtime mode.

In the same project view, click on the **Create** button and choose the **GitHub Repo** option. If you still haven't given GitHub permissions to Railway, choose the **Configure GitHub App** option.

![Configure GitHub Repo on Railway](https://res.cloudinary.com/craigsims808/image/upload/v1720439054/articles/medusa-sveltekit/railway-add-github-repo_cetjbf.png)

### Configure Medusa Backend Environment Variables

To configure the environment variables of your Medusa backend, click on the GitHub repo card and choose the **Variables** tab and add the following environment variables:

```
PORT=9000
JWT_SECRET=something
COOKIE_SECRET=something
DATABASE_URL=postgresql://dominggobana:JyyuEdr809p@df-hidden-bonus-ertd7sio.us-east-3.aws.neon.tech/storedb?sslmode=require
REDIS_URL=${{Redis.REDIS_URL}}
STORE_CORS=<CLOUDFLARE-STOREFRONT-URL>
ADMIN_CORS=<CLOUDFLARE-ADMIN-URL>
```

- `JWT_SECRET` and `COOKIE_SECRET` should be unique and secure values
- `DATABASE_URL` is the connection string to your Neon hosted Postgres database
- `REDIS_URL` references the value from the Redis database you created earlier.
- `STORE_CORS` and `ADMIN_CORS` should be replaced with the URL generated from the storefront and admin deployed on Cloudflare.

### Configure Start Command

Click on the GitHub repository's card, select the **Settings** tab and scroll down to the **Deploy** section. Click on the **Start** command button and paste the following command:

```sh
medusa migrations run && medusa start
```

![Change Start command in Railway](https://res.cloudinary.com/craigsims808/image/upload/v1720439054/articles/medusa-sveltekit/railway-change-start_zqbgyr.png)

### Deploy Changes

At the top left of your project's dashboard page, there's a **Deploy** button that deploys all the changes you've made so far.

![Railway Deploy button](https://res.cloudinary.com/craigsims808/image/upload/v1720439056/articles/medusa-sveltekit/railway-deploy-button_keobv6.png)

Click on the button to trigger the deployment. The deployment will take a few minutes before it's ready.

### Test the Backend

Once the deployment is finished, you can access the Medusa backend on the custom domain/domain you've generated.

For example, you can open the URL `<YOUR_APP_URL>/store/products` which returns the products available on your backend.

![Railway Deployment Test](https://res.cloudinary.com/craigsims808/image/upload/v1720439059/articles/medusa-sveltekit/railway-deploy-test_hempr1.png)

### Health Route

Access `<YOUR_APP_URL>/health` to get health status of your deployed backend.

![Raliway Deployment Health](https://res.cloudinary.com/craigsims808/image/upload/v1720439056/articles/medusa-sveltekit/railway-deploy-health_gv1vgs.png)

### Test the Storefront

Revisit your Cloudflare Dashboard and open the **Settings** page for your live storefront project. Update the value for environment variable `MEDUSA_BACKEND_URL` with the URL to your deployed Medusa backend server on Railway.

Save the settings and rebuild your storefront deployment.

Visit the URL to your deployed storefront in your browser. If all is working, you should see the home page populated with products.

![Storefront Starter on Cloudflare Pages]()

### Test the Admin

Revisit your Cloudflare Dashboard and open the **Settings** page for your live admin project. Update the value for environment variable `MEDUSA_ADMIN_BACKEND_URL` with the URL to your deployed Medusa backend server on Railway.

Save the settings and rebuild your admin deployment.

Visit the URL to your Medusa Admin and log in using the user you created in the previous steps.

![Medusa Admin on Cloudflare Pages](https://res.cloudinary.com/craigsims808/image/upload/v1720439036/articles/medusa-sveltekit/cloudflare-admin-login_xvkl9u.png)

If all is working you should be able to log in to your dashboard and see all the orders you made previously when testing the development version of your store.

![Medusa Admin Dashboard Logged In](https://res.cloudinary.com/craigsims808/image/upload/v1720439052/articles/medusa-sveltekit/medusa-admin-dashboard-loggedin-cloudflare_gxnlze.png)

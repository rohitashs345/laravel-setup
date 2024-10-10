
# Shopify Setup Template - PHP

This is a template for building a [Shopify app](https://shopify.dev/docs/apps/getting-started) using PHP and React. It includes the basic setup required for developing a Shopify app.

## Benefits

Shopify apps are developed using a range of Shopify tools to create an exceptional merchant experience. The [create an app](https://shopify.dev/docs/apps/getting-started/create) guide in our developer documentation will take you through the process of building a Shopify app using this template.

This PHP template offers out-of-the-box functionality for:

-   OAuth: Managing app installation and permission requests.
-   GraphQL Admin API: Interacting with Shopify admin data using queries or mutations.
-   REST Admin API: Classes to manage Shopify resources via the REST API.
-   Shopify-specific tools like:
    -   AppBridge
    -   Polaris
    -   Webhooks

## Tech Stack

This template integrates several open-source tools:

-   [Laravel](https://laravel.com/) is used for backend development and testing.
-   [Vite](https://vitejs.dev/) handles the build process for the [React](https://reactjs.org/) frontend.
-   [React Router](https://reactrouter.com/) is used for navigation, enhanced with file-based routing.
-   [React Query](https://react-query.tanstack.com/) is used to fetch data from the Admin API.
-   [`i18next`](https://www.i18next.com/) and associated libraries are used for internationalizing the frontend.
    -   [`react-i18next`](https://react.i18next.com/) provides React-specific i18n functionality.
    -   [`i18next-resources-to-backend`](https://github.com/i18next/i18next-resources-to-backend) dynamically loads app translations.
    -   [`@formatjs/intl-localematcher`](https://formatjs.io/docs/polyfills/intl-localematcher/) helps match the user locale to the app’s supported locales.
    -   [`@formatjs/intl-locale`](https://formatjs.io/docs/polyfills/intl-locale) serves as a polyfill for [`Intl.Locale`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Locale), if necessary.
    -   [`@formatjs/intl-pluralrules`](https://formatjs.io/docs/polyfills/intl-pluralrules) is a polyfill for [`Intl.PluralRules`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/PluralRules), if needed.

These third-party tools are enhanced with Shopify-specific utilities for easier app development:

-   [Shopify API library](https://github.com/Shopify/shopify-api-php) integrates OAuth for managing app installation and permission scopes in Laravel.
-   [App Bridge React](https://shopify.dev/docs/tools/app-bridge/react-components) handles authentication for API requests and renders components outside of the app’s iFrame.
-   [Polaris React](https://polaris.shopify.com/) offers a design system and component library to help developers create consistent, high-quality experiences for Shopify merchants.
-   [Custom hooks](https://github.com/Shopify/shopify-frontend-template-react/tree/main/hooks) provide access to authenticated requests for the GraphQL Admin API.
-   [File-based routing](https://github.com/Shopify/shopify-frontend-template-react/blob/main/Routes.jsx) simplifies the process of creating new pages.
-   [`@shopify/i18next-shopify`](https://github.com/Shopify/i18next-shopify) is a plugin for [`i18next`](https://www.i18next.com/) that enables translation files to follow the same schema as Shopify app extensions and themes.

## Getting Started

### Requirements

1. A [Shopify partner account](https://partners.shopify.com/signup) is required.
1. You’ll need a development store or [Shopify Plus sandbox store](https://help.shopify.com/en/partners/dashboard/managing-stores/plus-sandbox-store) for testing.
1. [PHP](https://www.php.net/) must be installed.
1. You’ll need [Composer](https://getcomposer.org/) for dependency management.
1. [Node.js](https://nodejs.org/) should be installed.

### Installing the Template

The template relies on Shopify CLI 3.0, which is a Node.js package you can install using any package manager:

With yarn:

```shell
yarn create @shopify/app --template php
```

Using npx:

```shell
npm init @shopify/app@latest -- --template php
```

Using pnpm:

```shell
pnpm create @shopify/app@latest --template php
```

This will clone the template and install the CLI in that project.

### Setting up your Laravel app

Once the Shopify CLI clones the repo, you will be able to run commands on your app.
However, the CLI will not manage your PHP dependencies automatically, so you will need to go through some steps to be able to run your app.
These are the typical steps needed to set up a Laravel app once it's cloned:

1. Start off by switching to the `web` folder:

    ```shell
    cd web
    ```

1. Install your composer dependencies:

    ```shell
    composer install
    ```

1. Create the `.env` file:

    ```shell
    cp .env.example .env
    ```

1. Bootstrap the default [SQLite](https://www.sqlite.org/index.html) database and add it to your `.env` file:

    ```shell
    touch storage/db.sqlite
    ```

    **NOTE**: Once you create the database file, make sure to update your `DB_DATABASE` variable in `.env` since Laravel requires a full path to the file.

1. Generate an `APP_KEY` for your app:

    ```shell
    php artisan key:generate
    ```

1. Create the necessary Shopify tables in your database:

    ```shell
    php artisan migrate
    ```

And your Laravel app is ready to run! You can now switch back to your app's root folder to continue:

```shell
cd ..
```

### Local Development

[The Shopify CLI](https://shopify.dev/docs/apps/tools/cli) connects to an app in your Partners dashboard.
It provides environment variables, runs commands in parallel, and updates application URLs for easier development.

You can develop locally using your preferred Node.js package manager.
Run one of the following commands from the root of your app:

Using yarn:

```shell
yarn dev
```

Using npm:

```shell
npm run dev
```

Using pnpm:

```shell
pnpm run dev
```

Open the URL generated in your console. Once you grant permission to the app, you can start development.

## Deployment

### Application Storage

This template uses [Laravel's Eloquent framework](https://laravel.com/docs/9.x/eloquent) to store Shopify session data.
It provides migrations to create the necessary tables in your database, and it stores and loads session data from them.

The database that works best for you depends on the data your app needs and how it is queried.
You can run your database of choice on a server yourself or host it with a SaaS company.
Once you decide which database to use, you can update your Laravel app's `DB_*` environment variables to connect to it, and this template will start using that database for session storage.

### Build

The frontend is a single page React app. It requires the `SHOPIFY_API_KEY` environment variable, which you can find on the page for your app in your partners dashboard.
The CLI will set up the necessary environment variables for the build if you run its `build` command from your app's root:

Using yarn:

```shell
yarn build --api-key=REPLACE_ME
```

Using npm:

```shell
npm run build --api-key=REPLACE_ME
```

Using pnpm:

```shell
pnpm run build --api-key=REPLACE_ME
```

The app build command will build both the frontend and backend when running as above.
If you're manually building (for instance when deploying the `web` folder to production), you'll need to build both of them:

```shell
cd web/frontend
SHOPIFY_API_KEY=REPLACE_ME yarn build
cd ..
composer build
```

## Hosting

When you're ready to set up your app in production, you can follow [our deployment documentation](https://shopify.dev/docs/apps/deployment/web) to host your app on a cloud provider like [Heroku](https://www.heroku.com/) or [Fly.io](https://fly.io/).

When you reach the step for [setting up environment variables](https://shopify.dev/docs/apps/deployment/web#set-env-vars), you also need to set the following variables:

| Variable          | Secret? | Required |     Value      | Description                                                                         |
| ----------------- | :-----: | :------: | :------------: | ----------------------------------------------------------------------------------- |
| `APP_KEY`         |   Yes   |   Yes    |     string     | Run `php web/artisan key:generate --show` to generate one.                          |
| `APP_NAME`        |         |   Yes    |     string     | App name for Laravel.                                                               |
| `APP_ENV`         |         |   Yes    | `"production"` |                                                                                     |
| `DB_CONNECTION`   |         |   Yes    |     string     | Set this to the database you want to use, e.g. `"sqlite"`.                          |
| `DB_DATABASE`     |         |   Yes    |     string     | Set this to the connection string to your database, e.g. `"/app/storage/db.sqlite"` |
| `DB_FOREIGN_KEYS` |         |          |     `true`     | If your app is using foreign keys.                                                  |

## Known issues

### Hot module replacement and Firefox

When running the app with the CLI in development mode on Firefox, you might see your app constantly reloading when you access it.
That happened in previous versions of the CLI, because of the way HMR websocket requests work.

We fixed this issue with v3.4.0 of the CLI, so after updating it, you can make the following changes to your app's `web/frontend/vite.config.js` file:

1. Change the definition `hmrConfig` object to be:

    ```js
    const host = process.env.HOST
        ? process.env.HOST.replace(/https?:\/\//, "")
        : "localhost";

    let hmrConfig;
    if (host === "localhost") {
        hmrConfig = {
            protocol: "ws",
            host: "localhost",
            port: 64999,
            clientPort: 64999,
        };
    } else {
        hmrConfig = {
            protocol: "wss",
            host: host,
            port: process.env.FRONTEND_PORT,
            clientPort: 443,
        };
    }
    ```

1. Change the `server.host` setting in the configs to `"localhost"`:

    ```js
    server: {
      host: "localhost",
      ...
    ```

### I can't get past the ngrok "Visit site" page

When you’re previewing your app or extension, you might see an ngrok interstitial page with a warning:

```
You are about to visit <id>.ngrok.io: Visit Site
```

If you click the `Visit Site` button, but continue to see this page, then you should run dev using an alternate tunnel URL that you run using tunneling software.
We've validated that [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/run-tunnel/trycloudflare/) works with this template.

To do that, you can [install the `cloudflared` CLI tool](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation/), and run:

```shell
# Note that you can also use a different port
cloudflared tunnel --url http://localhost:3000
```

In the output produced by `cloudflared tunnel` command, you will notice a https URL where the domain ends with `trycloudflare.com`. This is your tunnel URL. You need to copy this URL as you will need it in the next step.

```shell
2022-11-11T19:57:55Z INF Requesting new quick Tunnel on trycloudflare.com...
2022-11-11T19:57:58Z INF +--------------------------------------------------------------------------------------------+
2022-11-11T19:57:58Z INF |  Your quick Tunnel has been created! Visit it at (it may take some time to be reachable):  |
2022-11-11T19:57:58Z INF |  https://randomly-generated-hostname.trycloudflare.com                                     |
2022-11-11T19:57:58Z INF +--------------------------------------------------------------------------------------------+
```

In a different terminal window, navigate to your app's root and run one of the following commands (replacing `randomly-generated-hostname` with the Cloudflare tunnel URL copied from the output of `cloudflared` command):

```shell
# Using yarn
yarn dev --tunnel-url https://randomly-generated-hostname.trycloudflare.com:3000
# or using npm
npm run dev --tunnel-url https://randomly-generated-hostname.trycloudflare.com:3000
# or using pnpm
pnpm dev --tunnel-url https://randomly-generated-hostname.trycloudflare.com:3000
```

## Developer resources

-   [Introduction to Shopify apps](https://shopify.dev/docs/apps/getting-started)
-   [App authentication](https://shopify.dev/docs/apps/auth)
-   [Shopify CLI](https://shopify.dev/docs/apps/tools/cli)
-   [Shopify API Library documentation](https://github.com/Shopify/shopify-api-php/tree/main/docs)
-   [Getting started with internationalizing your app](https://shopify.dev/docs/apps/best-practices/internationalization/getting-started)
    -   [i18next](https://www.i18next.com/)
        -   [Configuration options](https://www.i18next.com/overview/configuration-options)
    -   [react-i18next](https://react.i18next.com/)
        -   [`useTranslation` hook](https://react.i18next.com/latest/usetranslation-hook)
        -   [`Trans` component usage with components array](https://react.i18next.com/latest/trans-component#alternative-usage-components-array)
    -   [i18n-ally VS Code extension](https://marketplace.visualstudio.com/items?itemName=Lokalise.i18n-ally)

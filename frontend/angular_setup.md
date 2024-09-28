# Setup Angular project

> We use angular Version 16 with SCSS

This guide provides the steps to set up an Angular project using Angular version 16, configure SCSS as the styling format, and ensure a smooth development experience.

## Prerequisites

Before starting the Angular setup, ensure that the following tools are installed:

-   **Node.js** (version 20.x)  
    [Download Node.js](https://nodejs.org/en/)

-   **npm** (comes with Node.js)  
    To check if you have npm installed, run:

    ```bash
    npm -v
    ```

-   **Angular CLI** (version 16)  
    Angular CLI is required to create and manage Angular projects. Install it globally using npm:

    ```bash
    npm install -g @angular/cli@16.2.14
    ```

## Step 1: Create a New Angular Project

Use the Angular CLI to create a new project. Replace `project-name` with your desired project name and ensure SCSS is selected as the styling format.

```bash
ng new project-name
```

When prompted:

-   Select **Yes** for Angular routing (`yes` or `no`).
-   Choose **SCSS** as the styling format (CSS, SCSS, Sass, etc.).

## Step 2: Navigate to the Project Directory

After creating the project, move into the project folder:

```bash
cd project-name
```

## Step 3: Start the Development Server

Start the built-in Angular development server with the following command:

```bash
ng serve
```

By default, the app will be available at `http://localhost:4200/`. Open this URL in a browser to view your Angular application.

To run the server on a different port, use the `--port` flag:

```bash
ng serve --port 4300
```

## Step 4: Project Structure Overview

After setting up an Angular project with SCSS, the default structure is as follows:

```
project-name/
├── e2e/              # End-to-end tests
├── node_modules/     # Installed dependencies
├── src/              # Application source code
│   ├── app/          # Components, modules, and services
│   ├── assets/       # Static assets (images, etc.)
│   ├── json/         # json files (configuration, etc.)
│   ├── environments/ # Environment-specific configurations
│   ├── index.html    # Application entry point
│   ├── main.ts       # Bootstrap file for the Angular app
│   ├── styles.scss   # Global SCSS styles
│   └── polyfills.ts  # Polyfill imports for browser compatibility
├── angular.json      # Angular project configuration
├── package.json      # Project dependencies and scripts
├── tsconfig.json     # TypeScript configuration
└── README.md         # Project documentation
```

The default global styles are in `src/styles.scss`. SCSS (Sassy CSS) allows you to write more modular and maintainable styles with features like variables, nesting, and mixins.

## Step 5: Installing Additional Dependencies

To install third-party libraries or tools, you can use npm.

For example, to add **Bootstrap**:

```bash
npm install bootstrap
```

Then import Bootstrap in `src/styles.scss`:

```scss
@import "~bootstrap/scss/bootstrap";
```

You can add your custom SCSS styles within `src/styles.scss` or create separate `.scss` files for individual components.

## Step 6: Building the Project

To compile the application for production, use the following command:

```bash
ng build --prod
```

The build output will be located in the `dist/` directory, and this folder can be deployed to a web server.

## Step 7: Environment Configuration

To manage different environments (e.g., development and production), configure settings in `src/environments/`. Angular allows you to create specific environment configurations for your project.

Build for production using:

```bash
ng build --prod
```

## Step 8: Git Setup (Optional)

To initialize a Git repository and version control your Angular project:

```bash
git init
git add .
git commit -m "Initial commit"
```

Add a `.gitignore` file to exclude unnecessary files, like `node_modules`, from version control:

```bash
node_modules/
dist/
```

## Configuration json

> We use configuration.json instead of environment.ts

All the api urls we maintain in configuration.json are defined in `src/assets/json/configuration.json`.

All the are defined in key value pairs in `configuration.json`.

### How to access the api url ?

-   Load the json file in main.ts file and export the file as a variable.

    ```typescript
    var tenant_configuration_path = "assets/json/configuration.json";
    export var configuration_json: any = {};

    fetch(tenant_configuration_path)
        .then((res) => res.json())
        .then((json) => {
            configuration_json = json;
        });
    ```

-   Use the configuration_json variable to access the api url.
    Make a service and add the function to get the api url based on the key.

    ```typescript
    import { configuration_json } from "src/main";
    ```

    ```typescript
    public get_config(key?: any) {
        if (key) {
        return configuration_json[key];
        } else {
        return configuration_json;
        }
    }
    ```

Or

-   We can directly import the variable `configuration_json` from `main.ts` in any component to get the api url

## Setup Angular project in WSL ubuntu

To setup an Angular project in WSL ubuntu, follow the steps below:

1. Install WSL Ubuntu

    > for more details, see [WSL Guid]()

2. Install Node.js and npm in WSL Ubuntu using

    [Install npm in Ubuntu](https://github.com/nodesource/distributions?tab=readme-ov-file#debian-and-ubuntu-based-distributions)

3. Install Angular CLI in WSL Ubuntu using command.

    ```bash
    sudo npm i -g @angular/cli@16.2.12
    ```

4. Check if Angular CLI is installed in WSL Ubuntu using command.

    ```bash
    ng -v
    ```

+++
title = "Frontend Development"
+++

HobbyFarm consists of two frontend applications: the [primary user interface (ui)](https://github.com/hobbyfarm/ui) and the [administrator user interface (admin-ui)](https://github.com/hobbyfarm/admin-ui). The UI is the primary application users interact with. The Admin UI is a separate application used to manage the HobbyFarm backend.

The following instructions will assist in setting up a local development environment for HobbyFarm.

## Requirements

Both the UI and Admin UI are written in [Angular](https://angular.io/). To develop either application, knowledge of Angular and Typescript is required.

The following tools are required to be installed:

* [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

## Setup

The following instructions will assist in setting up a local development environment for the UI and Admin UI. The instructions are the same for both applications.

1. Clone the UI and Admin UI repositories.
    ```bash
    ## Clone the UI repository
    git clone https://github.com/hobbyfarm/ui.git

    ## Clone the Admin-UI repository
    git clone https://github.com/hobbyfarm/admin-ui
    ```

2. Copy the example environment file and edit it. The example below uses the UI repository, but the same steps apply to the Admin-UI repository.
    ```bash
    ## Change into the UI directory.
    cd ui

    ## Copy the example environment file
    cp src/environments/environment.local.example.ts src/environments/environment.local.ts

    ## Edit the file
    vim src/environments/environment.local.ts
    ```

3. Set the `server` variable to the Gargantua backend URL in the `src/environments/environment.local.ts` file.
    ```typescript
    // Example src/environments/environment.local.ts
    export const environment = {
      production: false,
      // Update the server URL to point to the local gargantua backend
      server: 'http://localhost:16210',
    };
    ```

4. Launch the local server from the Git repository root directory.
    ```bash
    ## Run the install command
    npm install

    ## Start the local UI/Admin-UI server
    npm run start:local
    ```
![status](https://img.shields.io/badge/status-public-brightgreen.svg "App has been used as a basis for a public example. The app may continue to change as features are added.")

# SmartThings EndpointApp: Set the color of a light based on the weather.

This is a sample WebHook automation app using the SmartThings API.
This app showcases:

- App installation and configuration flow.
- HTTP Signature verification.
- Integrating with a third-party API (Weather API in this case).
- Actuating devices.
- Creating schedules and handling scheduled executions.

> NOTE: This example is illustrative in nature. It does not include anything like logging correlation or monitoring/metrics hooks, or other things that are required for production service deployments.

## Prerequisites

- [Node.js](https://nodejs.org/en/) and [npm](https://www.npmjs.com/) installed.
- [ngrok](https://ngrok.com/) installed to create a secure tunnel to create a globally available URL for fast testing.
- An account and location created in the environment you wish to install to, along with a SmartThings-compatible Color Control bulb.
- An API key from https://api.openweathermap.org (free tier is fine).

## Setup instructions

> NOTE: By default, this example uses the production API. If you wish to run this in the staging or dev environment, update the `apiUrl` value in `./config/default.json`.

### Add weather API key to config file

Replace the value of the weather API key in `./config/default.json` with your Open Weather Map API key.

### Start local server

1. `npm install`
2. `npm start`
3. In another console window, start ngrok: `ngrok http 3005`. Save the `https` URL somewhere, or copy to your clipboard.

### Create Automation in Workspace

1. Login to the Developer Workspace for your environment ([Staging](https://s-devworkspace.developer.samsung.com) or [Production](https://devworkspace.developer.samsung.com/smartthingsconsole/iotweb/site/index.html)).
2. Click "Automation" in left-side menu, then "Create".
3. Enter a unique value for "SmartApp Name" and provide a description.
4. For "SmartApp Type"  select "Webhook endpoint", and then enter the ngrok URL from above.
5. Select the scopes `r:devices:*`, `x:devices:*`, and `w:schedules`.
6. Click "Save and Next".
7. Enter a Display Name (e.g., "Weather Color Light Test").
8. Click "Confirm", then "Close".

The SmartApp is now self-published in the Catalog.

**IMPORTANT!**

**Before proceeding, copy the public key from the Self-Publish tab of the Automation in the Workspace, and replace the contents of `./config/smartthings_rsa.pub` with it.
Then stop and restart the server:**

- `CTRL-C`
- `npm start`

### Option 1: Install Automation in OneApp/SmartThings

1. Download and install the latest version of SmartThings developer build. Build downloads and instructions are found [here](https://smartthings.atlassian.net/wiki/spaces/SE/pages/129794483/Installing+Samsung+Connect+%28OneApp%29+Client+Binary).
2. Open SmartThings app, and select "Automations" tab.
3. Press "Add Automation".
4. Press the automation you registered in the Workspace and follow the installation instructions.
5. Once installed, the configured bulb should turn on and its color should either be purple (if precipitation is in the forecast), orange (if the forecast calls for temperatures above 80 degrees Fahrenheit), blue (if the forecast calls for temperatures below 50 degrees Fahrenheit), or white (if no precipitation and temperature between 50 and 80 degrees Fahrenheit). It will check the current weather at the interval selected by the user during installation.

### Option 2: Install Automation in Strongman

Strongman is the service that powers SmartApp configuration and installation.
The debug web view is particularly useful for debugging purposes, should you encounter errors during configuration/installation.

1. In the environment you created your SmartApp, go to the Strongman debug page ([DEV](https://strongman-regionald.smartthingsgdev.com/debug), [STAGING](https://strongman-regionals.smartthingsgdev.com/debug)).
2. Select the location you would like to install into, paste in the SmartApp ID (this can be found in the workspace for the Automation you created), and click _Submit_.
3. Follow the installation instructions.
4. Once installed, the configured bulb should turn on and its color should either be purple (if precipitation is in the forecast), orange (if the forecast calls for temperatures above 80 degrees Fahrenheit), blue (if the forecast calls for temperatures below 50 degrees Fahrenheit), or white (if no precipitation and temperature between 50 and 80 degrees Fahrenheit). It will check the current weather at the interval selected by the user during installation.

## Running tests

EndpointApps are services, and thus can and should contain automated tests.
To execute the automated tests for this app:

`npm test`

Run the lint check to ensure consistent code formatting and standards:

`npm run lint`

## Additional resources

### Librarian docs

Librarian is an app that houses the latest Swagger specifications that define the REST API.

You can view all the swagger projects here: https://librarian-regionals.smartthingsgdev.com/

Generally, EndpointApps need the following API references:

- [api.smartthings.com.v1](https://librarian-regionals.smartthingsgdev.com/api/api.smartthings.com.v1) (Base API that your SmartApp calls)
- [endpoint-apps.v1](https://librarian-regionals.smartthingsgdev.com/api/endpoint-apps.v1) (The contract of the payload your EndpointApp receies from SmartThings)

### Public docs

https://smartthings.developer.samsung.com

### Internal Troubleshooting docs

https://smartthings.atlassian.net/wiki/spaces/PI/pages/231112989/Troubleshooting

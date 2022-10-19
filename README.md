# Pause & Resume Call Recording Flex Plugin

## Notice

This plugin is no longer maintained as of October 17th, 2022. Work to maintain this feature in Flex V2 has been moved over to the [Twilio Professional Services Flex Project Template](https://github.com/twilio-professional-services/twilio-proserv-flex-project-template) where it is an [optional feature](https://github.com/twilio-professional-services/flex-project-template/tree/main/plugin-flex-ts-template-v2/src/feature-library/pause-recording/README.md)

## Flex Plugin

Twilio Flex Plugins allow you to customize the appearance and behavior of [Twilio Flex](https://www.twilio.com/flex). If you want to learn more about the capabilities and how to use the API, check out our [Flex documentation](https://www.twilio.com/docs/flex).

## How it works
This Flex plugin adds a Pause/Resume Recording button to the call canvas to allow the agent to temporarily pause the call recording before the customer provides payment information (such as credit card details, bank account, etc.) to the agent and to resume regular call recording afterwards.

This plugin leverages Twilio Functions to perform the actual Pause and Resume action on the call resource.

The application repo contains both a Flex Plugin project as well as a Twilio Serverless project. Deploy the Serverless functions before deploying the Flex plugin.

This Flex plugin incorporates the base Dual Channel Recording functionality from the [Flex Dual Channel Recording Solution](https://github.com/twilio-professional-services/flex-dual-channel-recording).
The call recordings are started from the plugin, leveraging a server-side Twilio Function to call the Twilio Recordings API. The task attribute `conversations.media` is updated with the recording metadata so Flex Insights can play the recording. Call recordings are dual-channel, capturing customer and agent audio in their own audio channels. This solution works for both inbound and outbound calls.

After deploying this plugin, you can disable the Call Recording setting in [Flex Settings](https://www.twilio.com/console/flex/settings) on the Twilio Console. This is the default single-channel (mono) Conference Call Recording which is now no longer required.


# Configuration

## Requirements

To deploy this plugin, you will need:

- An active Twilio account with Flex provisioned. Refer to the [Flex Quickstart](https://www.twilio.com/docs/flex/quickstart/flex-basics#sign-up-for-or-sign-in-to-twilio-and-create-a-new-flex-project%22) to create one.
- npm version 5.0.0 or later installed (type `npm -v` in your terminal to check)
- Node.js version 12 or later installed (type `node -v` in your terminal to check). We recommend the _even_ versions of Node.
- [Twilio CLI](https://www.twilio.com/docs/twilio-cli/quickstart#install-twilio-cli) along with the [Flex CLI Plugin](https://www.twilio.com/docs/twilio-cli/plugins#available-plugins) and the [Serverless Plugin](https://www.twilio.com/docs/twilio-cli/plugins#available-plugins). Run the following commands to install them:

```
# Install the Twilio CLI
npm install twilio-cli -g
# Install the Serverless and Flex as Plugins
twilio plugins:install @twilio-labs/plugin-serverless
twilio plugins:install @twilio-labs/plugin-flex
```

## Setup

Install the dependencies by running `npm install`:

```bash
cd pause-recording-plugin
npm install
cd ../serverless
npm install
```
From the root directory, rename `public/appConfig.example.js` to `public/appConfig.js`.

```bash
mv public/appConfig.example.js public/appConfig.js
```

## Serverless Functions

### Deployment

```bash
cd serverless
twilio serverless:deploy
```
After successfully deploying your functions, you should see at least the following:
```bash
✔ Serverless project successfully deployed
Functions:
   https://<your base url>/call-recording/create-recording
   https://<your base url>/call-recording/list-recordings
   https://<your base url>/call-recording/pause-recording
   https://<your base url>/call-recording/resume-recording
```

Your functions will now be present in the Twilio Functions Console and be part of the "pause-recording" service. Copy the base URL from one of the functions.

## Flex Plugin

### Development

Create the plugin config file by copying `.env.example` to `.env`.

```bash
cd pause-recording-plugin
cp .env.example .env
```

Edit `.env` and set the `REACT_APP_SERVICE_BASE_URL` variable to your Twilio Functions base URL (this will be available after you deploy your functions). In local development environment, it could be your localhost base URL.

To run the plugin locally, you can use the Twilio Flex CLI plugin. Using your command line, run the following from the root directory of the plugin.

```bash
cd pause-recording-plugin
twilio flex:plugins:start
```

This will automatically start up the webpack dev server and open the browser for you. Your app will run on `http://localhost:3000`.

When you make changes to your code, the browser window will be automatically refreshed.


### Deploy your Flex Plugin

Once you are happy with your Flex plugin, you have to deploy then release it on your Flex application.

Run the following command to start the deployment:

```bash
twilio flex:plugins:deploy --major --changelog "Releasing Pause & Resume recoding plugin" --description "Flex pause and resume recording"
```

After running the suggested next step, navigate to the [Plugins Dashboard](https://flex.twilio.com/admin/?_ga=2.87204752.948933873.1631534160-131677462.1626104298) to review your recently deployed plugin and confirm that it’s enabled for your contact center.

**Note:** Common packages like `React`, `ReactDOM`, `Redux` and `ReactRedux` are not bundled with the build because they are treated as external dependencies so the plugin will depend on Flex to provide them globally.

You are all set to test Pause and Resume Recording on your Flex application!

## License

[MIT](http://www.opensource.org/licenses/mit-license.html)

## Disclaimer

No warranty expressed or implied. Software is as is.

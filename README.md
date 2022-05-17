[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/wildbit/postmark_webhooks/tree/master)

<img width="148" height="148" align="right" alt="Postmark Webhooks Logo" src="https://newsletter.postmarkapp.com/assets/images/open-source/webhooks-logo@2x.png">

# Postmark Hooks

Brought to you by
<a href="http://postmarkapp.com"><img src="https://newsletter.postmarkapp.com/assets/images/pm_logo@2x.png" width="100" alt="Postmark"></a>

## About
This project is a quick start app you can use to host your own URLs for receiving, storing, processing, and viewing webhooks sent from [Postmark](http://postmarkapp.com). It is written in Meteor.js due to the framework's ability to quickly display server side data without needing a refresh in the browser. Be sure to read the [Postmark webhooks documentation](http://developer.postmarkapp.com/developer-webhooks-overview.html). Use the deploy to Heroku button (you can host it somewhere else if you want, like [Galaxy](https://www.meteor.com/hosting)) and begin taking advantage of Postmark's [Open tracking webhook](http://developer.postmarkapp.com/developer-open-webhook.html), [Bounce webhook](http://developer.postmarkapp.com/developer-bounce-webhook.html), [Click webhook](https://postmarkapp.com/developer/webhooks/click-webhook), [Delivery Webhook](http://developer.postmarkapp.com/developer-delivery-webhook.html), and [Inbound webhook](http://developer.postmarkapp.com/developer-inbound-webhook.html).

If you have not already done so, sign up for a Postmark account [here](https://account.postmarkapp.com/sign_up).

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17418107/95874d6c-5a4b-11e6-9608-6f0e03820bfb.gif)

## Features

- Receive webhook POSTs (well formatted JSON) from Postmark for Bounces, Opens, Clicks, Delivery events, and Inbound messages, with minimal development/configuration effort on your part.
- Automatically send notification emails (using Postmark) to yourself and/or others when you receive a new bounce, open, click, delivery event, or inbound message.
- Searchable so you can easily find and view details of a specific bounce, open event, click event, delivery event, or inbound message.

## Prerequisites

- [Postmark Account](https://account.postmarkapp.com/sign_up)
- [MongoDB Atlas Account](https://www.mongodb.com/cloud/atlas/register)
- [Heroku Account](https://signup.heroku.com)

## Getting Started

- Use the Deploy to Heroku button at the top of this page to quickly host your own instance of the app.
- Be sure to add a custom name for the app, rather than use one generated by Heroku:

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17383869/2bd1eb66-598d-11e6-81ea-688aa3bedb4a.png)

- Enter in your custom app's domain for `ROOT_URL`:

![alt tag](https://user-images.githubusercontent.com/16660335/98881443-ded4ea80-243e-11eb-9342-3c9342441fcd.png)

Make sure to update your Atlas db username, password, and database name in the connection string copied from Atlas.

- Create a database in Atlas and name it `webhooks`

- Add collections to the database in Atlas (`bounces`, `clicks`, `opens`, `deliveries`, `inbound`):

![alt tag](https://user-images.githubusercontent.com/16660335/98881478-f4e2ab00-243e-11eb-863c-b8d70a8122fe.png)

- Create an access user for the database in the Atlas cluster:

![alt tag](https://user-images.githubusercontent.com/16660335/104212459-184bb700-53ea-11eb-8392-b1e50d8da815.png)

- Use your MongoDB connection string from Atlas for the config variable `MONGODB_URI`:

![alt tag](https://user-images.githubusercontent.com/16660335/98881584-32473880-243f-11eb-8470-e8e741dc2484.png)

Make sure to replace the username/password for your DB access user and also update the <dbname> to match your database's name (webhooks) in the connection string.

- Once you have entered in your custom name, `MONGODB_URI`, and `ROOT_URL`, you can proceed to deploy the app to Heroku by clicking Deploy for Free. Once completed, click View to open your new app on the web.

![alt tag](https://cloud.githubusercontent.com/assets/16660335/17385099/43fc172c-5995-11e6-8833-7e4a4adb63fc.png)

## Configure Postmark Account

Our next step is to configure our server(s) in Postmark with the new URLs we have available for receiving webhooks. Setting these URLs tells in Postmark determines where to send the webhook events. Use the following URLs, where yourappname.herokuapp.com is replaced with your webhook receiving app's root domain.

- Bounces: https://yourappname.herokuapp.com/webhooks/bounces
- Opens: https://yourappname.herokuapp.com/webhooks/opens
- Clicks: https://yourappname.herokuapp.com/webhooks/clicks
- Delivery: https://yourappname.herokuapp.com/webhooks/delivered
- Inbound: https://yourappname.herokuapp.com/webhooks/inbound

## Set URL for each webhook type

From your server's *Webhooks* tab in Postmark, follow these steps for each webhook type you want to use.

1. Click *Add webhook*

![alt tag](https://user-images.githubusercontent.com/16660335/38372988-fab834ba-38a4-11e8-8124-f2b073b3bd6b.png)

2. Enter in the appropriate URL for that event (use your Bounces webhook URL when setting up the Bounces webhook, for example).

![alt tag](https://user-images.githubusercontent.com/16660335/38373082-31bb2bfc-38a5-11e8-8c24-0294fdf86619.png)

3. Click *Save webhook*

## Built With

* [Meteor.js](https://www.meteor.com)
* [Postmark.js](https://www.npmjs.com/package/postmark)

## Security

- Only [Postmark's listed IPs](http://support.postmarkapp.com/article/800-ips-for-firewalls) for webhooks can POST to URLs built by this app. Any other received request is ignored and no document is added to the collections in the database.
- Received data is also validated against a schema to ensure appropriate data received and stored as a document in its associated collection.
- Insecure removed so the database cannot be accessed from the client (browser dev tools)
- Option to receive notification emails if an unauthorized source attempts to post to your URL

## Modifying Your Application's Codebase

Once you have deployed your own instance of the application to Heroku, you will need to get a local version of the repository where you can make edits and push back to Heroku. If you simply clone your Heroku app's repo, you will end up with an empty repository locally. You can get a local copy of the source code using the following commands:

1. ``heroku git:clone -a YourAppName``
2. ``cd YourAppName``
3. ``git remote add origin https://github.com/pgraham3/postmark_webhooks``
4. ``git pull origin master``

Once you have the local repo for your application, you can then make changes and commit them in the usual fashion. When you are ready to push your local changes to your Heroku application, use this command:

``git push heroku master``

## Customize Your Notification Settings

In addition to receiving and processing Postmark webhooks, this app also allows for you to send emails using Postmark when you receive a bounce, open event, delivery event, or inbound message.

Grab the Server API Token for the Server you wish to send notifications from in Postmark (found in Credentials when viewing the server in Postmark) and add it as a Heroku config variable named POSTMARK_API_TOKEN. You can do this in the Heroku UI Settings for your app or using this command from the Heroku CLI:

``heroku config:set POSTMARK_API_TOKEN=YourTokenValue``

You can then send emails based on the following events:

* Bounce received
* Inbound Message received
* Open Event received
* Click Event received
* Delivery Event received
* Unauthorized (not from Postmark's IP addresses) POST received to one of your webhook URLs

Open up server/settings.js to view and modify the settings.

By default, notifications will **not** be sent for events unless you enable them by changing their value to ``true``. Sending notifications for events can be turned on/off for each of these event types by changing these fields to true/false:

* ``SendBouncesNotifications`` (for bounces)
* ``SendOpensNotifications`` (for open events)
* ``SendClicksNotifications`` (for click events)
* ``SendInboundNotifications`` (for inbound messages)
* ``SendViolationsNotifications`` (for unauthorized POSTs to your URLs)
```javascript
{
  "SendBouncesNotifications": false,
  "SendOpensNotifications": false,
  "SendClicksNotifications": false,
  "SendInboundNotifications": false,
  "SendDeliveredNotifications": false,
  "SendViolationsNotifications": false,
  "SendBouncesToSender": false,
  "BouncesFromEmailAddress": "pmnotifications+bounces@yourdomain.com",
  "OpensFromEmailAddress": "pmnotifications+opens@yourdomain.com",
  "ClicksFromEmailAddress": "pmnotifications+opens@yourdomain.com",
  "InboundFromEmailAddress": "pmnotifications+inbound@yourdomain.com",
  "DeliveredFromEmailAddress": "pmnotifications+delivered@yourdomain.com",
  "OpensToEmailAddress": "email@yourdomain.com",
  "ClicksToEmailAddress": "email@yourdomain.com",
  "InboundToEmailAddress": "email@yourdomain.com",
  "BouncesToEmailAddress": "email@yourdomain.com",
  "DeliveredToEmailAddress": "email@yourdomain.com",
  "ViolationsFromEmailAddress": "pmnotifications+violations@yourdomain.com",
  "ViolationsToEmailAddress": "email@yourdomain.com"
}
```

Set the From email addresses for your notifications to email addresses that you have added as Sender Signatures or are on your verified domains in Postmark to be sure you are able to send notification emails. These notification emails can be sent to any recipient.

## Note on Inbound Message Size Limits

MongoDB has an inherent limit of ~16.8 MB, which means documents (entries in a MongoDB collection) cannot be larger than this amount. If you receive an inbound webhook notification larger than 16.8 MB due to attachment size, it will not be stored in your collection or display in the Inbound tab and you will see a MongoDB error in your Heroku Logs:

``Error: Document exceeds maximum allowed bson size of 16777216 bytes``

## Authors

* **Patrick Graham**

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Meteor Community
* Mark Otto | Bootstrap
* Tom Coleman | Iron Router & Atmosphere
* Matteo De Micheli | Easy Search
* Charlie DeTar | Meteor.js buildpack for Heroku
* Rico Sta. Cruz | NProgress

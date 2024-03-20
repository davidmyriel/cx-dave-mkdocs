---
title: "Alert Webhook with GCP Chat"
date: "2021-02-02"
---

Configuring a Google chat webhook integration can easily be done with the custom webhook integration. Choose the WebHook integration and fill in your destination chat URL, you can check the documentation from Google [here](https://developers.google.com/hangouts/chat/how-tos/webhooks) to see how to retrieve the URL.

Next, define your webhook body. Note that Google chat API expects a flat JSON structure with one key "text" as the webhook body. It can still of course contain all the relevant information you are interested in from your log itself, by tagging the keys using '$' as explained above. Here is an example for you to test:

```
{"text": "Hi team! This is the Coralogix team, your webhook structure needs to be flat with one key in the JSON in order to fit Google chats. Use the Coralogix keys tagged with '$' to signify what you would like to send. Here is an example: alert_id=$ALERT_ID, name= $ALERT_NAME, description = $ALERT_DESCRIPTION, application = $APPLICATION_NAME  ,subsystem= $SUBSYSTEM_NAME, Alert Log = $LOG_TEXT  ------- You may see the above table containing all the different options you may use to structure your custom messages. Enjoy!"}
```

For more Google chat API options such as using formatted text in messages, including links in messages, @mention specific/all users you can visit [here](https://developers.google.com/hangouts/chat/reference/message-formats/basic).

When you are done configuring your desired webhook, In your [alert](https://coralogixstg.wpengine.com/tutorials/coralogix-user-defined-alerts/), go to the 'Notification settings" section and choose your newly defined webhook. 

\*\* If you don't see your new integration under your alert definition, try to refresh your browser

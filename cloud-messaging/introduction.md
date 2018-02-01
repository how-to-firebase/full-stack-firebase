# Firebase Cloud Messaging: Introduction

Cloud Messaging is used for those notifications that pop up in the corner of your Chrome or Firefox browser.

They're also used to pop native Android and iOS notifications.

Firebase Cloud Messaging (FCM) used to be known as Google Cloud Messaging (GCM), and if you're familiar with GCM... well, not much has changed.

FCM is advertised primarily for use with Android and iOS. The Firebase Dashboard doesn't even have an FCM page on it. There's a Notifications page for Android and iOS that encapsulates FCM functionality, but it's useless for web.

We'll be interacting directly with the FCM API using a Cloud Function. No Dashboard page is required.

### How do I send messages?

FCM is actually one of the simpler Firebase modules. 

You send messages using Cloud Functions or your own server, and you receive them with a service worker in your browser.

You can message individual browsers, and you can subscribe a browser to a "topic" so that it can receive bulk messages.

We'll get into the details later... just know that there's not much to it. You can send messages to your clients either individually or by topic. And the device handles displaying the message, so your work is done.

### Why send messages?

Excessive browser and mobile notifications are obnoxious. Our phones and browsers light up constantly with Twitter notifications trying to draw us back into the app. We disable those notifications pretty aggressively.

But there's a reason for the flood of alerts. They work. They pull people back into Twitter, Tumblr, and Facebook. And they're super helpful when you miss a bunch of Slack messages or emails from the office.

You're likely already sending notifications if you develop for Android or iOS. We won't be covering that here, but we'll show you how to send notifications to a web browser. Again, it's not complicated.

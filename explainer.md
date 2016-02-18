# Is App Installed Explained

# What's This All About?
This document looks into how a web developer would use the Is App Installed API, which lists website's native application which are installed on the user's device. This solves problems like how to deduplicate push notifications between [the web](https://developers.google.com/web/updates/2015/03/push-notificatons-on-the-open-web) and the native app. Currently, there is no satisfactory solution to avoid sending the notification twice, once to the website and once to the native app. By allowing the website to probe for the installation of the native app, the site can avoid sending the second notification.

# Describing a relationship from native application to website (and vice versa)
This API is being developed with the assumption that a system exists to create associations from applications to web applications.

We can define relationships between a web application and other applications by using the "related_applications" member of a web application.

# Querying the installed local apps that specify the website.

```js
var isAppInstalledPromise = navigator.listInstalledApps();
isAppInstalledPromise.then(function(listOfInstalledApps) {
    // success
    for (var app of listOfInstalledApps) {
        console.log(app.platform);
        console.log(app.url);
        console.log(app.id);
    }
}, function() {
    // failure
});
```

In this example, we ask for a list of installed apps which are associated with our web application and receive a Promise, which will resolve to the apps which are locally installed. In order to receive any results, the web application must define the "related_applications" member of its web app manifest.

The promise is resolved with the locally installed applications, provided that they associate with the web application. Each platform has its own method of verifying a relationship. In Android, the [Digital Asset Links](https://developers.google.com/digital-asset-links/v1/create-statement) system can be used to define an association between a website and an application. If the application is installed locally and defines an association with the requesting web application, we return the app as defined in the "related_applications" member.

# Notes

This feature only works with sites using HTTPS. This ensures that the website cannot be spoofed, and that the association between the site and application is valid.

This feature will return that every application is not installed if the browser is in Incognito Mode. This ensures that the browser does not accidentally leak any information in Incognito Mode.

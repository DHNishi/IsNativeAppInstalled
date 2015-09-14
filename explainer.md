# IsNativeAppInstalled Explained

# What's This All About?
This document looks into how a web developer would use the IsNativeAppInstalled API, which says if a website's native application is installed on the user's device. This solves problems like how to deduplicate push notifications between [the web](https://developers.google.com/web/updates/2015/03/push-notificatons-on-the-open-web) and the native app. Currently, there is no satisfactory solution to avoid sending the notification twice, once to the website and once to the native app. By allowing the website to probe for the installation of the native app, the site can avoid sending the second notification.

# Describing a relationship from native application to website (and vice versa)
This API is being developed with the assumption that a system exists to create associations between native applications and websites. Such a system requires both the app to define a relationship to the website and the website to define a relationship to the app.

# Querying the native app installation status.

```js
var isAppInstalledPromise = IsNativeAppInstalled.isAppInstalled("com.example.myapp", ["VERIFICATION_KEY", "OTHER_VERIFICATION_KEY"]);
isAppInstalledPromise.then(function(isAppInstalled) {
    // success
    if (!isAppInstalled) {
        // do something
    }
}, function() {
    // failure
});
```

In this example, we probe the installation status of an example app. In order to verify that the Android app package name corresponds to the correct Android app, we also provide the signatures included with the package. This is to avoid a situation of a package name collision causing an incorrect response.

If the application is not installed or if the application is installed but an association could not be verified between the site and the app, the promise is fulfilled with False.

If the application is installed and an association can be proven between the website, the promise is fulfilled with True.

# Notes

This feature only works with sites using HTTPS. This ensures that the website cannot be spoofed, and that the association between the site and application is valid.

This feature will return that every application is not installed if the browser is in Incognito Mode. This ensures that the browser does not accidentally leak any information in Incognito Mode.

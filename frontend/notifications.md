# Notifications

The application supports two types of notifications:

1. Desktop notifications
2. App notifications

Both notifications can be used through the `NotificationManager`.

## Desktop Notifications

Desktop notifications make use of the `Notification` Api. These notifications allow to inform the user in the browser and desktop. With push notifications it's possible to send notifications directly to the user without waiting for the user to request outstanding notifications. This can be achieved by making use of `Service Workers`. The push notifications are especially helpful for mobile users to inform them about changes in the app (e.g. new message, new task etc.).

... service worker implementation coming soon ...

```js
notifyManager.send(
    new NotificationMessage(
        NotificationStatus.SUCCESS,
        'Title',
        'My Message'
    ),
    NotificationType.BROWSER_NOTIFICATION
);
```

## App notifications

App notifications are a custom notification implementation which can be themed and manipulated within the app. These notifications are most helpful for direct user interactions/feedback (e.g. giving the user feedback if the input was successful or not).

```js
notifyManager.send(
    new NotificationMessage(
        NotificationStatus.SUCCESS,
        'Title',
        'My Message'
    ),
    NotificationType.APP_NOTIFICATION
);
```
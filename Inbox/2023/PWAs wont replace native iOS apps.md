---
created: 2024-02-19T21:02:03 (UTC -03:00)
tags: []
source: https://app.campsite.co/campsite/p/notes/rengvq2txami?utm_source=tldrnewsletter
author: 
---

# PWAs wont replace native iOS apps

> ## Excerpt
> Last month, we shipped PWA support for Campsite after tons of user requests for a mobile app. Having Campsite on our home screens and receiving push notifications immediately convinced us it was worth it, and our users were happy.

---
Last month, we shipped PWA support for [Campsite](https://www.campsite.co/) after tons of user requests for a mobile app. Having Campsite on our home screens and receiving push notifications immediately convinced us it was worth it, and our users were happy.

We _really_ want a native app for Campsite, but we’re still a small team on the hunt for product-market fit. Adding more platform overhead would slow us down.

At the same time, we’ve found that supporting a PWA on iOS takes a ton of time — no matter how hard we try, there are too many bugs and missing features for us to ever feel like we’ve met our bar for craft and quality.

## Web Push: the good

Apple added [Safari Web Push to PWAs in iOS 16.4](https://developer.apple.com/documentation/usernotifications/sending_web_push_notifications_in_web_apps_and_browsers). This API is incredibly easy to use — you don’t even need a developer account.

Campsite is a Rails API with a NextJS frontend. All we needed to do to get web push working:

1.  Detect if we’re running in a PWA environment via `window.matchMedia('(display-mode: standalone)')` and prompt to enable push.
    
2.  We request push permissions and register a service worker. [Pushpad’s service worker](https://pushpad.xyz/service-worker.js) helped us get started. The worker sends the registered push endpoint and p256dh & auth tokens to our API.
    
3.  When a pushable event happens, we queue a Sidekiq job to send a push using [Pushpad’s forked web-push gem](https://github.com/pushpad/web-push). If Apple returns a 410 response, we destroy the push subscription.
    

And with that, we can send pushes for high-signal notifications and chats in Campsite.

## Web Push: the bad

If you’re used to building native push notifications, there will be a lot of missing features and gotchas with Safari Web Push.

The most surprising thing we found is that you must always display the notification on the device when it is received. This is done in the service worker, so you technically _could_ skip displaying it. But if you do, Apple will revoke your push endpoint. From the [docs](https://developer.apple.com/documentation/usernotifications/sending_web_push_notifications_in_web_apps_and_browsers#3975534):

> _Present push notifications to the user immediately after your service worker receives them. If you don’t, Safari revokes the push notification permission for your site._

[Users on Reddit](https://www.reddit.com/r/PWA/comments/127p7s1/ios_pwa_web_push_notification_behaviours/) found that permissions were revoked the third time a notification wasn’t displayed.

That means that if you’re viewing a chat thread on your PWA and you get a new message, you can’t suppress the new-message push. This is incredibly annoying for users.

We work around this by delaying sending notifications by 10 seconds so that chats can be marked read while you look at them. Native apps can control whether or not to display a push.

Here’s a bunch of other frustrations we have with Safari Web Push on iOS:

-   There are no [silent pushes](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/pushing_background_updates_to_your_app), so we can only update the app icon badge with a displayed push. Ideally, if you clear your notifications elsewhere, we automatically remove the badge on your phone. This isn’t possible with PWAs via push.
    
-   Notifications can’t be removed from notification center unless you tap on them. You can [get notifications](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerRegistration/getNotifications), but calling `notification.close()` does nothing. Users have to clear Campsite pushes manually.
    
-   You can’t remove the “from Campsite” byline.
    
-   There is no [notification grouping](https://developer.apple.com/documentation/watchos-apps/grouping-notifications) or [communication notifications](https://medium.com/orion-innovation-techclub/whats-new-in-ios-15-notifications-7486e712df40).
    

## Bugs & missing features

We have dynamic light and dark mode support in Campsite driven by [next-themes](https://github.com/pacocoursey/next-themes) and simply:

```
window.matchMedia('(prefers-color-scheme: dark)')
```

While your PWA will render in light or dark mode, **it will not switch until the app is killed**.

This broke with the release of iOS 17. That was over 6 months ago as of writing this post, and it appears broken in the 17.4 beta still. Developers have been reporting issues [for](https://www.reddit.com/r/webdev/comments/178073i/ios_17_bug_pwas_no_longer_react_to_darklight_mode/#) [some](https://forums.developer.apple.com/forums/thread/739154) [time](https://stackoverflow.com/questions/77450440/the-event-listener-added-by-window-matchmediaprefers-color-scheme-dark-is).

There are a few more annoying bugs that we run into frequently:

-   We use CSS sticky to keep elements pinned to the top/bottom of the viewport (nav bars, chat input, etc). After a while, sticky elements break and freely scroll around the page. This is only fixed by killing the app.
    
-   The keyboard is incredibly fickle, too. It pushes sticky elements off the screen, and sometimes it just stops appearing in the PWA, and the only fix is to restart your phone.
    
-   There are no APIs for haptics, PIP, or contact sync. ([Thanks for pointing these out Jacob](https://twitter.com/therealjfrantz/status/1758212307128119563))
    

## Where do we go from here?

I stand by our decision to build a PWA. We discovered that app icon badges and push notifications help us stay more connected. Our users agree — some are now spending half their time on Campsite from their phones!

Unfortunately, PWAs on iOS don’t meet our bar for craft and quality at Campsite, and the latest news about [Apple restricting PWA use in Europe](https://www.macrumors.com/2024/02/08/ios-17-4-nerfs-web-apps-in-the-eu/) is also concerning. We fully intend on building a native app as soon as we lock in Campsite’s core features.

If your users are asking for a native app, building basic PWA support will help you learn quickly. Just don’t expect your PWA to come close to a native experience.

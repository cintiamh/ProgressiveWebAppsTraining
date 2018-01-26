# ProgressiveWebAppsTraining

PWA training from: 
* https://developers.google.com/web/ilt/pwa/
* https://www.youtube.com/watch?v=17kGWJOuL-A&amp;list=PLNYkxOF6rcIAdnzEsWkg0KpMn2WJwMBmN

* Who are my users?
    * Brosers
    * OSs
    * connectivity
    * offline?
    * data cost
* Understanding requirements
    * Performance
    * Content
    * Functionality
    
AMP + service worker

* [Technologies](#technologies)
* [Introduction to service workers](#introduction-to-service-workers)

## Technologies

### Fetch & Promises

fetch API example:
```javascript
fetch('/examples/example.json')
    .then(function(response) {
        return response.json();
    }) // .then do something with the data
    .catch(function(error) {
        console.log('Fetch failed', error);
    });
```

## Introduction to service workers

A service worker is a client-side programmable proxy between your web app and the outside world.

It gives you fine control over network requests.

Service workers are a type of web worker, an object that executes a script separately from the main browser thread.

Service workers run independent of the application they are associated with and can receive messages when not active, either because your application is in the background or not open, or the browser is closed.

Main uses for service workers:
* act as a caching agent
* handle network requests
* store content for offline use
* handle push messaging

Service worker becomes idle when not in use and restarts when needed.

If there is information you need to persist and reuse across restarts, then service workers can work with IndexDB databases.

Service workers are promise-based.

Service workers also depend on two APIs to work effectively:

* fetch: a standard way to retrieve content from the network
* cache: a persistent content storage for application data. (persistent and independent from browser cache or network status)

Service workers are only available on secure origins served through TLS, using the HTTPS protocol and localhost.

Services like Let's encrypt SSL for free.

### Application shell architecture

Cache common assets for your application (HTML, CSS, JS, images, etc).

Service workers can act as base for advanced features:

* channel messaging API: allows web workers and service workers to communicate with each other and with the host application.
    * new content notifications
    * updates that require user interaction
* notifications API: way to integrate push notifications from the application to the operating system's native notification system.
* push API: enables push services to send push messages to an application. This service can send messages at any time, even when the application or the browser is not running.
    * push notifications and sent to a service worker that can use the information in the message to update local state, or display a notification to the user.
* Background sync lets you defer actions until the user has stable connectivity.
    * this API also allows servers to push periodic updates to the app so the app can update when it's next online.
    
### Service worker lifecycle

Registration => Installation => Activation

#### Registration/Installation

```javascript
if (!('serviceWorker' in navigator)) {
    console.log('sw not supported');
    return;
}
navigator.serviceWorker
    .register('./service-worker.js')
    .then(function(registration) {
        console.log('SW registered! Scope is: ', registration.scope);
    });
    // .catch()
```

Include this in your application entry point.

The browser will only registrate your service worker if it is new or has been updated.

```javascript
navigator.serviceWorker.register(
    '/service-worker.js',
    { scope: '/app/' }
);
```

The scope of the service worker determines from which path the service worker will intercept requests.
The default scope is the folder where the service-worker file is located and all directories below it.

A service worker cannot have a scope above its own path.

#### Service worker file's events

```javascript
self.addEventListener('install', function(event) {
    // Do stuff during install    
});
```

Once the browser register the service worker, the `install` event occurs.

The `install` happens when the service worker is new, or if it is different from one registered before.

The `install` event is a good time to do the caching of the app shell or static assets using the cache API.

```javascript
self.addEventListener('activate', function(event) {
    // Do stuff during install
});
```
8:30
# Google Analytics Primer

----

## Implementation Snippet

``markup
<!-- Google Analytics -->
<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','//www.google-analytics.com/analytics.js','ga');
ga('create', 'UA-XXXX-Y', 'auto');
ga('send', 'pageview');
</script>
<!-- End Google Analytics -->
``

## What does the implementation do?

> The JavaScript snippet above initially defines the ga function as a simple queue. All arguments passed to it are stored in an internal array. Once the analytics.js library is loaded, each call in the queue is executed and the ga function is redefined so all subsequent calls execute immediately. Initializing the ga function as a queue gives developers a consistent API to use regardless of whether the analytics.js library has finished loading.

## Modifying Implementation

- You can rename the global object by changing the last argument of the initialiser ('ga')
- There's an alternative implementation that uses *async* script loading but downgrades for IE9. Recommended for primarily modern browser projects.

## The Tracker

- Tracker objects collect and send data to you GA account
- A tracker is initialised *in place*, since most sites will only have one tracker.

``javascript
ga('create', 'UA-XXXX-Y', 'auto');
``

- _Initialisation will store all the relevant metrics and will use this for their default values._ This is why updating the tracker location is the recommended solution for SPA pageview tracking.

``javascript
// Set a single field.
ga('set', 'page', '/about');
// Set multiple fields.
ga('set', {
  page: '/about',
  title: 'About Us'
});
``

- You can retrieve tracker data from GA:

``javascript
ga(function(tracker) {
  var defaultPage = tracker.get('page');
});
``

- You can use multiple trackers:

``javascript
ga('create', 'UA-XXXX-Y', 'auto'); // creates an unnamed, default tracket
ga('create', 'UA-12345-6', 'auto', {'name': 'newTracker'});  // New (named) tracker.
//
ga('send', 'pageview'); // sends pageview to the default tracket
ga('newTracker.send', 'pageview'); // Sends pageview to the new tracker
``

## Create Method

``javascript
ga('create', 'UA-XXXX-Y', 'auto', {}); // args: method, web property ID, [cookie domain configuration], [customisation options]
``

- Test GA form locahost by passing the *cookieDomain* option attribute value 'none';
- Force SSL by passing the *cookieDomain* option attribute value true;

## Dimensions & Metrics

- Used to send business data associated with a hit
- Dimensions are strings and Metrics are numbers
- Passed via option key *dimension\[0-9\]+* and *metric\[0-9\]+* (max 20 for free acounts)

``javascript
// Examples
ga('set', 'dimension5', 'custom data');
// or
ga('send', 'event', 'category', 'action', {
  'metric18': 8000
});
// or
ga('send', 'pageview', {
  'dimension15':  'My Custom Dimension'
}); 
``

## Page Tracking

![GA Page Tracking](-JoAYoIZOTMRWGeAHNWJ)

``javascript
ga('send', 'pageview', {
  'page': '/my-overridden-page?id=1',
  'title': 'my overridden page'
});
``

## Event Tracking

Measure how users interact with the page

![GA Event Tracking](-Jo5PpjgTUe7IbGzupgq)

``javascript
ga('send', 'event', 'button', 'click', 'nav buttons', 4);
``

## Exception Tracking

``javascript
ga('send', 'exception', {
  'exDescription': 'DatabaseError',
  'exFatal': false,
  'appName', 'myApp',
  'appVersion', '1.0'
});

## Plugins

``markup
<script async src="myplugin.js"></script>
``

``javascript
// Tracker initialization
ga('create', 'UA-XXXXX-Y', 'auto');
ga('require', 'myplugin');
ga('send', 'pageview');
``

``javascript
// Plugin definition
//
// Provides a plugin name and constructor function to analytics.js. This
// function works even if the site has customized the ga global identifier.
function providePlugin(pluginName, pluginConstructor) {
  var ga = window[window['GoogleAnalyticsObject'] || 'ga'];
  if (ga) ga('provide', pluginName, pluginConstructor);
}
// Plugin constructor.
function MyPlugin(tracker) {
  alert('Loaded myplugin on tracker ' + tracker.get('name'));
}
// Register the plugin.
providePlugin('myplugin', MyPlugin); // handles async nature of loads
``

**Config**

``javascript
// Importing 
ga('require', 'localHitSender', {path: '/log', debug: true});
// Implementation
function LocalHitSender(tracker, config) {
  this.url = config.url;
  this.debug = config.debug;
  if (this.debug) {
    alert('Enabled local hit sender for: ' + this.url);
  }
}
providePlugin('localHitSender', LocalHitSender);
``

**Custom Methods**

``javascript
// Generic
ga('[targetName.][pluginName:]methodName', ...);
``

``javascript
// Execute the 'doStuff' method using the 'myplugin' plugin.
ga('create', 'UA-XXXXX-Y', 'auto');
ga('require', 'myplugin');
ga('myplugin:doStuff');
// Execute the 'setEnabled' method of the 'hitCopy' plugin on tracker 't3'.
ga('create', 'UA-XXXXX-Y', 'auto', {name: 't3'});
ga('t3.require', 'hitcopy');
ga('t3.hitcopy:setEnabled', false);
``

``javascript
// myplugin constructor.
var MyPlugin = function(tracker) {
};
// myplugin:doStuff method definition.
MyPlugin.prototype.doStuff = function() {
  alert('doStuff method called!');
};
// hitcopy plugin.
var HitCopy = function(tracker) {
};
// hitcopy:setEnabled method definition.
HitCopy.prototype.setEnabled = function(isEnabled) {
  this.isEnabled = isEnabled;
}:
``

## SPA Considerations

- Updating the tracker page value is the recommended approach to getting the routing info right. You can do it at the hit level as well, but you would have to repeat the same values for event or social hits.

``javascript
ga('set', {
  page: '/new-page',
  title: 'New Page'
});
``

- DO NOT:
  - Use a different tracker to emulate a refresh
  - Change the tracker location value*
  - Change the tracker referee value*

\* Internally, GA will adjust your navigation paths from the pageview hits you send.

## Social Interaction Tracking

![Social Interaction Tracking](-JoAgHHJy6awBR52KJ53)

``javascript 
FB.Event.subscribe('edge.create', function(targetUrl) {
  ga('send', 'social', 'facebook', 'like', targetUrl);
});
``

## User ID Setting

``javascript
ga('create', 'UA-XXXX-Y', { 'userId': 'USER_ID' });
ga('send', 'pageview');
``

## Tasks

- Tasks process the measurement protocol request for transmition (ie. validation, checks, building payload, sending etc).
- You can't really create a new task, but you can chain your functionality to an existing entry, for instance:

``javascript
ga('create', 'UA-XXXXX-Y', 'auto');
ga(function(tracker) {
  // Grab a reference to the default sendHitTask function.
  var originalSendHitTask = tracker.get('sendHitTask');
  // Modifies sendHitTask to send a copy of the request to a local server after
  // sending the normal request to www.google-analytics.com/collect.
  tracker.set('sendHitTask', function(model) {
    originalSendHitTask(model);
    var xhr = new XMLHttpRequest();
    xhr.open('POST', '/localhits', true);
    xhr.send(model.get('hitPayload'));
  });
});
ga('send', 'pageview');
``

- You can abort processing by throwing an exception:

``javascript
ga('create', 'UA-XXXXX-Y', 'auto');
ga(function(tracker) {
  var originalBuildHitTask = tracker.get('buildHitTask');
  tracker.set('buildHitTask', function(model) {
    if (document.cookie.match(/testing=true/)) {
      throw 'Aborted tracking for test user.';
    }
    originalBuildHitTask(model);
  });
});
ga('send', 'pageview');
``

- You can disable a task by setting its value to null.

## User Timing Tracking

![User Timing](-JoD0QVPPYxjqFczihuf)

``javascrtipt
var startTime;
function loadJs(url, callback) {
  var js = document.createElement('script');
  js.async = true;
  js.src = url;
  var s = document.getElementsByTagName('script')[0];
  js.onload = callback;
  startTime = new Date().getTime();
  s.parentNode.insertBefore(js, s);
}
function trackTimingCallback(event) {
  var endTime = new Date().getTime();
  var timeSpent = endTime - startTime;
  ga('send', 'timing', 'jQuery', 'Load Library', timeSpent, 'Google CDN');
  // Library has loaded. Now you can use it.
};
``

----

# The Platform 

TBC: https://developers.google.com/analytics/devguides/platform/


# Google Analytics Primer

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

## Create Method

``javascript
ga('create', 'UA-XXXX-Y', 'auto'); // args: method, web property ID, [cookie domain configuration\], [options\]
``

## Event Tracking

Measure how users interact with the page

![GA Event Tracking](-Jo5PpjgTUe7IbGzupgq)

``javascript
ga('send', 'event', 'button', 'click', 'nav buttons', 4);
``
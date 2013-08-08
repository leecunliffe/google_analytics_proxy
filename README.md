google_analytics_proxy
======================

Google Analytics for Firefox Add-on SDK

You might want to add Google Analytics to your add-on in the panel module. For example, to track usage or interaction with specific elements of your add-on.

However, there is a bug in the sdk that means ga.js won't work (https://bugzilla.mozilla.org/show_bug.cgi?id=785914).

This small library provides a work-around, by using an iframe proxy. It opens an iframe on your server, then communicates with it using HTML5's window.postMessage.



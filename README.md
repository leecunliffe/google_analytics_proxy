google_analytics_proxy
======================

Google Analytics for Firefox Add-on SDK

You might want to add Google Analytics to your add-on in the panel module. For example, to track usage or interaction with specific elements of your add-on.

However, there is a bug in the sdk that means ga.js won't work (https://bugzilla.mozilla.org/show_bug.cgi?id=785914).

This small library provides a work-around, by using an iframe proxy. It opens an iframe on your server, then communicates with it using HTML5's window.postMessage.

To use:

On your server, put the ga_xdcom.html file somewhere so it is accessable at something like http://www.example.com/ga_xdcom.html

You'll need to edit this file, to set the event.origin so that only you can communicate with your proxy.

    // edit this line to use your add-on ID, found in package.json
    if (event.origin == "resource://YOU-AT-YOUR-EXTENTION"


In your extension, put the ga_proxy.js file in you /data directory.

In lib/main.js, you might have something like this:

    var panel = require("panel").Panel({
      contentURL: data.url("panel.html")
    });
    
    
panel.html:

    <!DOCTYPE html>
    <html>
    <head>
      <script type="text/javascript" src="ga_proxy.js"></script>
      <script type="text/javascript">
        var _gaq = new GaProxy("http://www.example.com/ga_xdcom.html");
        //Now, you can use the _gaq api as per normal, 
        //and the requests will be proxied through to the iframe hosted on your server.        
        _gaq.push(['_setAccount', 'UA-YOUR-ID'],["_setDomainName", "none"]);
        _gaq.push(['_setAllowHash', false]);
        _gaq.push(['_trackPageview', '/'+initial_page]);
      </script>
      


## Chapter 7 - AJAX

### 0. Preface

Ajax, at its most basic level, is a way of communicating with a server without unloading the current page; data can be requested from the server or sent to it.
Major Requesting Data Ways:

- XHR (XMLHttpRequest)
- Dynamic script tag insertion
- Multipart XHR
- ...

### 1. XMLHttpRequest

MDN doc: [link](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

- A readyState of 4 indicates that the entire response has been received and is available for manipulation.
- Data is still being transferred by listening for readyState 3. This is known as streaming.

```
req.onreadystatechange = function() {
    if (req.readyState === 3) { // Some, but not all, data has been received.
        var dataSoFar = req.responseText;
        ...
    }
    else if (req.readyState === 4) { // All data has been received.
        var data = req.responseText;
        ...
    }
}
```

### 2. Dynamic script tag insertion

This technique overcomes the biggest limitation of XHR: it can request data from a server on a different domain. It is a hack.

```
var scriptElement = document.createElement('script');
scriptElement.src = 'http://any-domain.com/javascript/lib.js';
document.getElementsByTagName('head')[0].appendChild(scriptElement);

function jsonCallback(data) {

    // Process the data here...
}
```

> Important:
> Because the response is being used as the source for a script tag, it must be executable JavaScript. You cannot use bare XML, or even bare JSON; any data, regardless of the format, must be enclosed in a callback function.

Eg: the `lib.js` file would enclose the data in the `jsonCallback()` function:

```
jsonCallback({ "status": 1, "colors": [ "#fff", "#000", "#ff0000" ] });
```

### 3. Multipart XHR

The newest of the techniques mentioned here, multipart XHR (MXHR) allows you to pass multiple resources from the server side to the client side using only one HTTP request.

#### 3.1 How to implement it?

This is done by packaging up the resources (whether they be CSS files, HTML fragments, JavaScript code, or base64 encoded images) on the server side and sending them to the client as a long string of characters, separated by some agreed-upon string.

On the clinet side the data is processed by:

```
var imageData = imageString.split("\u0001");
```

#### 3.2 How to process data on client side better?

As MXHR responses grow larger, it becomes necessary to process each resource as it is received, rather than waiting for the entire response. This can be done by listening for readyState 3:

- using a variable `lastLenghth` to record on local to read data
- set an interval to read `in_processing_data` when `readyState === 3`
- clear the interval and read last package when `readyState === 4`

```
var req = new XMLHttpRequest();
var getLatestPacketInterval, lastLength = 0;

req.open('GET', 'rollup_images.php', true);
req.onreadystatechange = readyStateHandler;
req.send(null);

function readyStateHandler{
    if (req.readyState === 3 && getLatestPacketInterval === null) {

        // Start polling.

        getLatestPacketInterval = window.setInterval(function() {
            getLatestPacket();
         }, 15);
    }

    if (req.readyState === 4) {

        // Stop polling.

        clearInterval(getLatestPacketInterval);

        // Get the last packet.

        getLatestPacket();
    }
}

function getLatestPacket() {
    var length = req.responseText.length;
    var packet = req.responseText.substring(lastLength, length);

    processPacket(packet);
    lastLength = length;
}
```

#### 3.3 Online Example

The code required to use MXHR in a robust manner is complex but worth further study. The complete library can be easily be found online at [http://techfoolery.com/mxhr/](http://techfoolery.com/mxhr/).

### 4. Beacons

#### 4.1 Old Javascript way

JavaScript is used to create a new Image object, with the src set to the URL of a script on your server. This URL contains the data we want to send back in the GET format of key-value pairs. Note that no img element has to be created or inserted into the DOM.

```
var url = '/status_tracker.php';
var params = [
    'step=2',
    'time=1248027314'
];

(new Image()).src = url + '?' + params.join('&');
```

**Pros:**
Beacons are the fastest and most efficient way to send data back to the server. The server doesn’t have to send back any response body at all, so you don’t have to worry about downloading data to the client.

**Cons:**
you are restricted in what you can do.

- You can’t send POST data, so you are limited to a fairly small number of characters before you reach the maximum allowed URL length.
- You _can_ receive data back, but in very limited ways.

#### 4.2 New: beacon API

https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon

### 5. Data Formats

- XML
- XPath
- Json: a lightweight and easy-to-parse data format written using JavaScript object and array literal syntax.
- [JSON-P](https://www.w3schools.com/js/js_json_jsonp.asp): In order to accomplish this, the data must be wrapped in a callback function. This is known as “JSON with padding,” or JSON-P. Here is our user list formatted as JSON-P:
  ```
  parseJSON([
      { "id": 1, "username": "alice", "realname": "Alice Smith",
          "email": "alice@alicesmith.com" },
      { "id": 2, "username": "bob", "realname": "Bob Jones",
          "email": "bob@bobjones.com" }
  ]);
  ```

### 6. Cache data - improve ajax performance

- On the server side, set HTTP headers that ensure your response will be cached in the browser.
- On the client side, store fetched data locally so that it doesn’t have be requested again.

#### 6.1 setting HTTP headers

The Expires header tells the browser how long a response can be cached.

#### 6.2 store locally

Instead of relying on the browser to handle caching, you can also do it in a more manual fashion, by storing the responses you receive from the server.

```
var localCache = {};
function xhrRequest(url, callback) {
    // Check the local cache for this URL.
    if (localCache[url]) {
        callback.success(localCache[url]);
        return;
    }
    // ...
}
```

### 7. Summary

In addition to these formats and transmission techniques, there are several guidelines that will help your Ajax appear to be faster:

- Reduce the number of requests you make, either by concatenating JavaScript and CSS files, or by using MXHR.
- Improve the perceived loading time of your page by using Ajax to fetch less important files after the rest of the page has loaded.
- Ensure your code fails gracefully and can handle problems on the server side.
- Know when to use a robust Ajax library and when to write your own low-level Ajax code.

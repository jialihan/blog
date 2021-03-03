## HTTP CORS Headers

### 1. what is Cross-Origin Resource Sharing (CORS) ?

**Cross-Origin Resource Sharing**  ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an  [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)-header based mechanism that allows a server to indicate any other  [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin)s (domain, scheme, or port) than its own from which a browser should permit loading of resources.

An example of a cross-origin request: the front-end JavaScript code served from `https://domain-a.com` uses [`XMLHttpRequest`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) to make a request for `https://domain-b.com/data.json`.

- Same Domain: always allow
- Diff Domain: cors controll

### 2. [Simple requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests "Permalink to Simple requests")

One of the allowed methods:

-   [`GET`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)
-   [`HEAD`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD)
-   [`POST`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)

The only headers which are allowed to be manually set are [those which the Fetch spec defines as a “CORS-safelisted request-header”](https://fetch.spec.whatwg.org/#cors-safelisted-request-header), which are:
-   [`Accept`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept)
-   [`Accept-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language)
-   [`Content-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)
-   [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
The only allowed values for the [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) header are:
	-   `application/x-www-form-urlencoded`
	-   `multipart/form-data`
	-   `text/plain`

Example 1: [upload json data](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#uploading_json_data) 
```js
fetch('https://example.com/profile', {
  method: 'POST', // or 'PUT'
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data),
})
```

**Example 2**: [upload a file](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#uploading_a_file) through `<input />` field
```js
const formData = new FormData();
const fileField = document.querySelector('input[type="file"]');

formData.append('username', 'abc123');
formData.append('avatar', fileField.files[0]);

fetch('https://example.com/profile/avatar', {
  method: 'PUT',
  body: formData
})
```


### 3.[The HTTP response headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers "Permalink to The HTTP response headers")

#### 3.1 [Access-Control-Allow-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#access-control-allow-origin "Permalink to Access-Control-Allow-Origin")

The `**Access-Control-Allow-Origin**` response header indicates whether the response can be shared with requesting code from the given [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin).

**Example 1:** 
A response that tells the browser to allow code from any origin to access a resource will include the following:
```js
Access-Control-Allow-Origin: *
```

**Example 2:**  
A response that tells the browser to allow requesting code from the origin `https://developer.mozilla.org` to access a resource will include the following:
```js
Access-Control-Allow-Origin: https://developer.mozilla.org
```

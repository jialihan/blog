## Integrate Google Map API in HTML

#### I. [Map Interface Choices](#p1)  

#### II. [Get started: Integrate google map API](#p2)  

#### III. [Init Map function](#p3)

#### IV. [HTML  Geolocation API ](#p4) 

#### V. [Custom Markers](#p5)

#### VI. [Add Re-Center Event on google map](#p6)

<div id="p2" />

### I. Map Interface choices
- google map API
- [Leaflet](https://leafletjs.com/) for Javascript
- [d3.js](https://github.com/d3/d3/blob/master/API.md#geographies-d3-geo): Geographies (d3-geo)
<div id="p2" />

### II. Guideline: how to start?

Docs: [integrate googlemap in html](https://developers.google.com/maps/documentation/javascript/adding-a-google-map#page)

#### 1.1 add Google Map script tag
Note: -   If  `async`  is present: The script is executed asynchronously with the rest of the page (the script will be executed while the page continues the parsing)
```html
<script
src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap&libraries=&v=weekly"
async
></script>
```

#### 1.2 Create a HTML map element
```html
<div  id="map"></div>
```

#### 1.3 Get your API key
follow the google docs: [get your API key - link](https://developers.google.com/maps/documentation/javascript/adding-a-google-map#key)


<div id="p3" />

### III. [Init Map function](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure)

San Diego geo loction: `{ lat:  -25.344, lng:  131.036  }`.

```js
// Initialize and add the map  
function initMap()  {  
	// The location of Uluru  
	const uluru =  { lat:  -25.344, lng:  131.036  };  
	// The map, centered at Uluru  
	const map =  new google.maps.Map(document.getElementById("map"),  
	{ 
		zoom:  4, 
		center: uluru,
	});  
	// The marker, positioned at Uluru  
	const marker =  new google.maps.Marker(
	{ 
		position: uluru,
		map: map,  
	});  
}
```

<div id="p4" />

### IV. HTML  Geolocation API 

Docs: [geolocation api - mdn](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)

```js
function  getLocation() {
	if (navigator.geolocation) {
	navigator.geolocation.getCurrentPosition(showPosition);
	} else {
	console.log("Geolocation is not supported by this browser.");
	}
}

function  showPosition(position) {
	loc[0] = position.coords.latitude;
	loc[1] = position.coords.longitude;
	// recenter the map with current location
	map.setCenter(new  google.maps.LatLng(loc[0], loc[1]));
	console.log("Latitude: " + loc[0] +
	", Longitude: " + loc[1]);
}
```


<div id="p4" />

### V. Custom Markers

Docs: [custom-markers](https://developers.google.com/maps/documentation/javascript/custom-markers)

**Default** place a marker at position:
```js
// The marker, positioned at Uluru
const marker = new google.maps.Marker({
	position: uluru,
	map: map,
});
```

Marker with **custom image icon:**
```js
const  image =
"https://developers.google.com/maps/documentation/javascript/examples/full/images/beachflag.png";
const  marker = new  google.maps.Marker({
	position:  uluru,
	map,
	icon:  image
});
```

<div id="p6" />

### VI. Add Re-Center Event on google map

#### 6.1 add re-center button into Map UI
Note: this script is parsing asynchronous, so document maybe not ready, we need to use DOM-Manipulation to **create & add this new element.**

```js
function  addReCenterButtonControl() {
	var  reCenterBtnEL = document.createElement('button');
	reCenterBtnEL.id = "recenter-btn";
	reCenterBtnEL.textContent = 'Re-Center';
	reCenterBtnEL.classList.add('recenter-btn');
	
	// also add event listener on this element
	reCenterBtnEL.addEventListener('click', function (e) {
		map.setCenter({ lat:  loc[0], lng:  loc[1] });
		reCenterBtnEL.style.display = 'none';
	});
	
	// Add this button into google map UI control
	map.controls[google.maps.ControlPosition.TOP_CENTER].push(reCenterBtnEL);
}
```

#### 6.2 Show this button Only when Center-Changed
Docs: [handling events - google map api](https://developers.google.com/maps/documentation/javascript/events)

Add `center_changed` event in this google map:
```js
google.maps.event.addListener(map, 'center_changed', function (event) {
	document.getElementById('recenter-btn').style.display = 'block';
});
```


### VII. Result

![image](../assets/googlemap_with_recenter.png ':size=685x495')
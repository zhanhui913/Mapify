# Mapify
Basic map/marker functionality but on an image.

Author: Zhan Yap<br/>
License: MIT

##Usage
```html
	
	//Include jQuery
	<script type="text/javascript" src="js/jquery/jquery.js"></script>

	//Include jQuery-UI for drag/drop
	<script type="text/javascript" src="js/jquery/jquery-ui.js"></script>

	//Include toe.js from https://github.com/visiongeist/toe.js
	//toe.js is a tiny library based on jQuery to enable sophisticated gestures on touch devices
	<script type="text/javascript" src="js/toe.min.js"></script>

	//Include jQuery Mousewheel
	<script type="text/javascript" src="js/jquery.mousewheel.min.js"></script>

	//Include imgViewer from https://github.com/waynegm/imgViewer
	<script type="text/javascript" src="js/imgViewer.min.js"></script>

	//Include the CSS
	<style type="text/css" media="all">@import "css/marker.css";</style>
```

Put an image element and a javascript block to attach the plugin to the image in the DOM
```html
	<body>
	    ...
	    <img  class="image" src="example.png"/>              
	    ...
	    <script type="text/javascript">
	        (function($) {
	        	$(document).ready(function() {
	            	$(".image").mapify();
	        	});
	        })(JQuery);
	    </script>
	    ...
	</body>

```

##Options
---
###zoomStep
* How much the zoom changes for each mousewheel click (positive number)
* Default : 0.1
* Example : To change each mousewheel click to 0.15 
```html
    $(".image").mapify("option", "zoomStep", 0.15);
```

###zoom
* Get/Set the current zoom level of the image (>=1)
* Default : 1 (ie the entire image is visible) 
* Example : Display the image magnified 4X
```html
    $(".image").mapify("option", "zoom", 4);
```

###zoomable
* Boolean to control if the image is zoomable
* Default : true
* Example : To disable image zooming
```html
    $(".image").mapify("option", "zoomable", false);
```

###editMode
* Boolean to control if the image is editable or view only.
* Default : false
* Example : To enable edit mode
```html
    $(".image").mapify("option", "editMode", true);
```

###vAll
* The vertical center point of the marker image.
* Default : "middle"
* Example : Change the center point to "bottom"
```html
    $(".image").mapify("option", "vAll", "bottom");
```

###hAll
* The horizontal center point of the marker image.
* Default : "middle"
* Example : Change the center point to "right"
```html
    $(".image").mapify("option", "hAll", "right");
```

**Important** : vAll and hAll is used concurrently to determine the center point of the marker image.

**Example** : 
![Marker](/img/markerPosition.png)
* **1** : {vAll: "top", hAll: "left"}
* **2** : {vAll: "top", hAll: "middle"}
* **3** : {vAll: "top", hAll: "right"}
* **4** : {vAll: "middle", hAll: "left"}
* **5** : {vAll: "middle", hAll: "middle"}
* **6** : {vAll: "middle", hAll: "right"}
* **7** : {vAll: "bottom", hAll: "left"}
* **8** : {vAll: "bottom", hAll: "middle"}
* **9** : {vAll: "bottom", hAll: "right"}

###topLatLng
*  The top left corner of the image's latitude & longitude
*  Default : {lat : 0, lng : 0}
*  Example : Set the top left corner of the image's latitude and longitude to 49.285546 and -123.119395 respectively.
```html
    $(".image").mapify("option", "topLatLng", {
        lat: 49.285546,
        lng : -123.119395
    });
```

###botLatLng
*  The bottom right corner of the image's latitude & longitude
*  Default : {lat : 1, lng : 1}
*  Example : Set the bottom right corner of the image's latitude and longitude to 49.284271 and -123.117394 respectively.
```html
    $(".image").mapify("option", "botLatLng", {
        lat: 49.284271,
        lng : -123.117394
    });
```

###markersLatLng
* An array of markers to be inserted upon document ready into the image. The "note" property is the tooltip when hovering over the marker, it can be ignored.
* Default : [{lat : 0.5, lng : 0.5, note : "Default note."}]
* Example : Add 2 markers.
```html
    $(".image").mapify("option", "markersLatLng", [
        {lat : 49.284893 , lng : -123.118376, note : "Intersection"},
        {lat : 49.285524 , lng : -123.118718, note : "Cactus Club" }
    ]);
```

###onClick
* Callback triggered by a mouseclick on the image
* Default : null
* Callback arguments :
    * e : the original click event object
    * self :  the mapify widget object issuing the click event
* Example : Adding a marker to the spot where the cursor was clicked.
```html
    $(".image").mapify("option", "onClick", function(e, self){
        var rpos = self.cursorToImg(e.pageX, e.pageY);
        self.addMarker(rpos.x, rpos.y);
        self.options.onUpdate.call(self);
    });
```

###onUpdate
* Callback triggered after the image has been redisplayed (repainted)
* Default : null
* Callback arguments :
    * e : always null
    * self: the mapify widget object issuing the update notice
* Example : To display the relative image coordinates clicked (relative image coordinates ranges from 0 to 1 where the top left is (0,0) and bottom right is (1,1))
```html
    $(".image").mapify("option", "onUpdate", function(e, self){
        var pos = {
                    relx : self.vCenter.x/$(self.img).width(),
                    rely : self.vCenter.y/$(self.img).height()
                    };
        $("#centre_position").html(pos.relx + "," + pos.rely);
    });
```

###onDragStart
* jQuery's onDragStart event callback when start dragging a marker.
* Default : null
* Example : Logging to the console a message.
```html
    $(".image").mapify({
        ...
        onDragStart: function(){
            console.log("Override for on drag start.");
        }
        ...
    });
```

###onDrag
* jQuery's onDrag event callback when dragging a marker.
* Default : null
* Example : Logging to the console a message.
```html
    $(".image").mapify({
        ...
        onDrag: function(){
            console.log("Override for on dragging.");
        }
        ...
    });
```

###onDragStop
* jQuery's onDragStop event callback when stop dragging a marker.
* Default : null
* Example : Logging to the console a message.
```html
    $(".image").mapify({
        ...
        onDragStop: function(){
            console.log("Override for on drag stop.");
        }
        ...
    });
```
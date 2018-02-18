
1.  We are now ready to put together the three different components of your of your geological career, which are
    1. Your geocoded work locations in GeoJSON format (done previously by you)
    1. The 'HTML' code needed to display your data as an interactive webpage with a Google Maps background
    1. The geological symbols that will replace the default 'red teardrop' Google Map point symbol
>

2.  All of the code that you will need to produce has to be done using a simple text editor (such as Notepad in Windows, TextEdit for Apple, Gedit for Linux) as wordprocessors tend to put in all sorts of hidden formatting stuff that JavaScript does not  like.

3.  Anyway once you have downloaded your GeoJSON file from **[the Google Geocoder](https://google-developers.appspot.com/maps/documentation/utils/geojson/)**, it should look something like this (here for example representing two individual reference points, one traverse line, and a concession area) 


```JSON:.geojson
{
  "type": "FeatureCollection",
  "features": [

    {
      "type": "Feature",
      "geometry": { "type": "Point",
       "coordinates": [ 144.49284296482801,  -16.9885020653736  ]
      },
      "properties": {}
    },
 
    {
      "type": "Feature",
      "geometry": { "type": "Point",
       "coordinates": [ 144.51284296482801,  -17.1885020653736  ]
      },
      "properties": {}
    },

    {
      "type": "Feature",
      "geometry": {  "type": "LineString",
        "coordinates": [
         [ 144.38572626560926,   -17.334908065808026 ],
          [ 144.61369257420301,  -17.334908065808026  ]
        ]
      },
      "properties": {}
    },

    {
      "type": "Feature",
      "geometry": {  "type": "Polygon",
        "coordinates": [
          [
            [ 144.44615107029676,  -17.114542657151983  ],
            [ 144.63017206639051,  -17.11716759748718 ],
            [ 144.63291864842176,  -17.27721873578159 ],
            [ 144.43791132420301,   -17.26672782352052 ],
            [ 144.44615107029676,   -17.114542657151983 ]
          ]
        ]
      },
      "properties": {}
    }

  ]
}
```
You now need to save it as a JSON file and call it something like **cv-job.json**

4.  You need to be aware however that the geographical coordinates from the **Geocoder** have a couple of _"gotchas"_

      1. Ridiculous levels of accuracy (15 places of decimals !),  but you only need to have a maximum of 5 places of decimals (as one degree equates to around 111 km, hence 5 places of decimals gives an accuracy of around 1 metre)

      1. The geocoder gives results as Longitude, Lattitude not the usual way round for a Geologist !

      1. If you are geocoding a polygon, the first and last set of coordinates have to be identical in order to close it off correctly

5.  You will then have to open your saved file using a text editor and fill out and edit the **properties** part yourself, much like this -

```JSON:.geojson
    "properties":
   {
    "icon": "au-a.svg",
    "country": "Australia",
    "location": "Charters Towers",
    "employer": " Red Dome",
    "work": "Grassroots exploration for",
    "commodity": "Gold",
    "geology": "http://www.mininglegacies.org/mines/queensland-2/red-dome/"
   }
  ```  
6.  If you look at this example as it appears in its final form **[on a Google Map](http://zaknbur.github.io/cv-jobs/cv-job-map-1.html)**  you will noticed that Google's default "red upside down tear-drop" point symbol has been converted into a more geologically significant symbol. The way this is done via the **"icon"** tag by creating an *SVG* image (in this case for **Gold**, called **au.svg**)  that looks like this.  

  ```  
<svg xmlns="http://www.w3.org/2000/svg" width="44" height="44">
<circle cx="22" cy="22" r="20" opacity="1.0" 
    style="  stroke: black;
	     stroke-width: 1;
           fill:gold"/>
  <text x="0" y="30"
      style="font-family: Times New Roman ;
             font-size: 30px;
             stroke: black;
	     stroke-width: 1;
             fill: white;">
    Au
  </text>
</svg>
  ```  
7. The only bits you need to change (to indicate the particular metal / mineral you were looking for at that location) are 
  - firstly the colour which is on the fifth line after fill:  i.e. **gold** , and 
  - secondly the third line from the bottom i.e. **Au**

So for example if you had been exploring for Copper, you would change the chemical symbol from _**Au to Cu**, and the fill colour from **gold to green**

8. I have included a bunch of these metal / mineral SVG files in this sub-directory, which also includes the 'HTML' code needed to display your data as an interactive webpage with a Google Maps background, your GeoJSON file should also go in here as well, as it is much simpler to keep everything together in one place when you copy the files to your own website / Github.io account.

9. The 'HTML' code will need to be changed by you in four places, which as you will see in the final block of code are as follows.
   - your webpage Title
   - your GeoJSON file-name - if you did not save it as cv-job.json
   - your start location for the map to be displayed
   - your Google API key
 
The places where you need to change the 'HTML code are shown in the comments lines which are shown as 
     `<!--COMMENT--> ` and  `//COMMENT// `

  ```
<!DOCTYPE html>
<html>
  <head>
  
<!-- COMMENT =   CHANGE AND PUT IN YOUR OWN NAME AND WEBPAGE TITLE--> 

    <title>YOUR NAME - WEBPAGE TITLE</title>
    <meta name="viewport" content="initial-scale=1.0">
    <meta charset="utf-8">
    <style>
      html, body {
        height: 100%;
        margin: 0;
        padding: 0;
      }
      #map {
        height: 100%;
      }
      #info {
        background-color: white;
        border: 1px solid black;
        bottom: 70px;
        height: 22px;
        padding: 5px;
        position: absolute;
        left: 20px;
  	opacity: 0.6;
	font-size:20px;
     }
      #fieldview {
        background-color: white;
        border: 1px solid blue;
        bottom: 40px;
        height: 20px;
        padding: 5px;
        position: absolute;
        left: 40px;
  	opacity: 0.6; 

      }
      #geolmap {
        background-color: white;
        border: 1px solid red;
        bottom: 10px;
        height: 20px;
        padding: 5px;
        position: absolute;
        left: 60px;
  	opacity: 0.6;
	}

    </style> 
  </head>
  <body>
    
    <div id="map"></div>
    <div id="info">commodity and location</div>
    <div id="fieldview">worked for</div>
    <div id="geolmap">Web link</div>

    <script>

var map;

// COMMENT =  CHANGE StartLocation COORDINATES

var StartLocation = {lat: -7.1049, lng: 38.1863};

function initMap() {
  map = new google.maps.Map(document.getElementById('map'), {
    zoom: 6,
    center: StartLocation,
  mapTypeId: 'terrain',
  });

  // COMMENT  =  IF NECESSARY CHANGE GeoJSON FILE-NAME
  
  map.data.loadGeoJson('cv-job.json');
  
  // Add some style.
  map.data.setStyle(function(feature) {
  var icon=null;
  if (feature.getProperty('icon')) {
    icon = feature.getProperty('icon');
  }
    return /** @type {google.maps.Data.StyleOptions} */({
      fillColor: feature.getProperty('fill'),
      strokeWeight: 1,
    icon: icon
    });
  });
  
  // SET mouseover event [TABLET] or click event [LAPTOP] for each feature.
  map.data.addListener('click', function(event) {
    document.getElementById('info').textContent = event.feature.getProperty('commodity')+' in   '+event.feature.getProperty('country');
    }); 

  map.data.addListener('click', function(event) {
    document.getElementById('fieldview').textContent = event.feature.getProperty('work')+'    '+event.feature.getProperty('location') +' for    '+event.feature.getProperty('employer');
    }); 

    map.data.addListener('click', function(event) {
    document.getElementById('geolmap').innerHTML ='<a href="http://'+event.feature.getProperty('reference')+'"target="_blank">Geology</a>';      
   });

    </script>

  // COMMENT  =  SUBSTITUTE YOUR OWN API KEY after initMap&&key=
  
    <script async defer
        src="https://maps.googleapis.com/maps/api/js?callback=initMap&&key=AIzaSyBVe7eSqeJTEczY7ctqonFWW-iLomIVYYI" async defer></script>
  </body>
</html>
  ```

<!DOCTYPE html>
<html>
  <head>
    <title>St Barbara - Concessions Norway</title>
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
      #info-box {
        background-color: white;
        border: 1px solid black;
        bottom: 70px;
        height: 20px;
        padding: 5px;
        position: absolute;
        left: 20px;
  	opacity: 0.6;
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
      #text {
        background-color: #ffffff;
        border: 1px solid red;
        top: 45px;
        height: 25px;
        padding: 1px;
        position: absolute;
        left: 15%;
  	opacity: 0.75;
  	filter: alpha(opacity=60); /* For IE8 and earlier */
  
      }
    </style> 
  </head>
<body>
 <center><b style="font-size:20px"><a href="http://geo.ngu.no/kart/mineralressurser/?Box=114117:6597635:158956:6645539" target="_blank">Norwegian - Concessions (NGU website)</a></b> - 
   <i><a href="http://www.norgeskart.no/#!?project=norgeskart&layers=1002&zoom=7&lat=7041543.63&lon=270409.26&sok=trond" target="_blank"> - Topo maps</a> - 
     <a href="https://minit.dirmin.no/kart/" target="_blank"> - Mining maps</a></i> </center> 
    <div id="map"></div>
    <div id="info-box">location details</div>
    <div id="fieldview">data sheets</div>
    <div id="geolmap">concession geology map</div>
    

 
    <script>

var map;
var auluz = {lat: 30.75, lng: -8.25};

function initMap() {
  map = new google.maps.Map(document.getElementById('map'), {
    zoom: 11,
    center: auluz
  });

  // Load GeoJSON.
  map.data.loadGeoJson('maroc-coords.json');
  
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

  // [START snippet]
  // Set mouseover event for each feature laptop, click for tablet.
  map.data.addListener('click', function(event) {
    document.getElementById('info-box').textContent = event.feature.getProperty('name')+',  '+event.feature.getProperty('commodity');
    }); 

    map.data.addListener('click', function(event) {
    document.getElementById('fieldview').innerHTML ='<a href="http://aps.ngu.no/pls/oradb/minres_deposit_fakta.Main?p_objid='+event.feature.getProperty('reference')+'"target="_blank">Site factsheet</a>'+','+event.feature.getProperty('UTM coords');
   }); 

    map.data.addListener('click', function(event) {
    document.getElementById('geolmap').innerHTML ='<a href="/'+event.feature.getProperty('geology')+'"target="_blank">Geology</a>'; 
   });
}
    </script>

    <script async defer
        src="https://maps.googleapis.com/maps/api/js?callback=initMap&&key=AIzaSyBVe7eSqeJTEczY7ctqonFWW-iLomIVYYI" async defer></script>
  </body>
</html>

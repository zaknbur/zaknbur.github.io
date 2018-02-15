
1. All of the code that you will need to produce has to be done using a simple text editor (such as Notepad in Windows, TextEdit for Apple, Gedit for Linux) as wordprocessors tend to put in all sorts of hidden formatting stuff that JavaScript does not  like.

2. Anyway once you have downloaded your GeoJSON file from **[the Google Geocoder](https://google-developers.appspot.com/maps/documentation/utils/geojson/)**, it should look something like this (here for example representing two individual reference points, one traverse line, and a concession area) -

a
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
You now need to save it as a JSON file and call it something like **job-cv.json**

3. You need to be aware however that the geographical coordinates from the **Geocoder** have a couple of _"gotchas"_

      1. Ridiculous levels of accuracy (15 places of decimals !),  but you only need to have a maximum of 5 places of decimals (as one degree equates to around 111 km, hence 5 places of decimals gives an accuracy of around 1 metre)

      1. The geocoder gives results as Longitude, Lattitude not the usual way round for a Geologist !

     1. If you are geocoding a polygon, the first and last set of coordinates have to be identical in order to close it off correctly

4. You will then have to open your saved file using a text editor and fill out and edit the **properties** part yourself, much like this -

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
5. If you look at this example as it appears in its final form **[on a Google Map](http://zaknbur.github.io/cv-jobs/cv-job-map-1.html)**  you will noticed that Google's default "red upside down tear-drop" point symbol has been converted into a more geologically significant symbol. The way this is done via the **"icon"** tag by creating an *SVG* image (in this case for **Gold**, called **au.svg**)  that looks like this.  

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
The only bits you need to change (to indicate the particular metal / mineral you were looking for at that particular location) are 
       * firstly the colour which is on the fifth line after fill:  i.e. **gold** ,and 
       * secondly the third line from the bottom i.e. **Au**

So for example if you had been looking for Copper, you would change the chemical symbol from _**Au to Cu**, and the fill colour from **gold to green**

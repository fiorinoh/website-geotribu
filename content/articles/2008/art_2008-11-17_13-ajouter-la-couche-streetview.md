---
authors:
- Fabien Goblet
categories:
- article
date: 2008-11-17 10:20
description: '...'
image: ''
license: default
robots: index, follow
tags:
- Google Maps
- Street View
title: 13. Ajouter la couche StreetView
---

# 13. Ajouter la couche StreetView


:calendar: Date de publication initiale : 17 novembre 2008


----





### Introduction




---


L'API Google Maps propose le service Street View qui permet de naviguer virtuellement dans les rues. Avant de voir comment faire dans le prochain tutoriel, nous allons voir ici quelle est l'emprise des images.  



### Initialisation




---


Reprendre la carte du [tutoriel n°2](http://www.geotribu.net/node/13).  



### Superposition de l'emprise




---


Construire un overlay de type StreetView :  

`var svOverlay = new GStreetviewOverlay();`  

L'ajouter à la carte :  

`map.addOverlay(svOverlay);`  



### Code complet




---


`-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">  







[Google Maps] 13. Ajouter la couche StreetView  



html { overflow:hidden; height:100%; }
body { height:100%; margin:0; }
#map { width:100%; height:100%; }



var map = null;

function initialize() {
if (GBrowserIsCompatible()) {
var map = new GMap2(document.getElementById('map'));
map.setCenter(new GLatLng(43.57691664771851, 1.402451992034912),15);
map.addControl(new GMapTypeControl());
map.removeMapType(G\_HYBRID\_MAP);
map.addMapType(G\_PHYSICAL\_MAP);
map.setMapType(G\_PHYSICAL\_MAP);
map.addControl(new GOverviewMapControl());
map.addControl(new GScaleControl());
map.addControl(new GLargeMapControl());
map.enableScrollWheelZoom();
var svOverlay = new GStreetviewOverlay();
map.addOverlay(svOverlay);

}
else{
alert('Désolé, mais votre navigateur n\'est pas compatible avec Google Maps');
}
}`  



### Démonstration




---






[Résultat pleine page](http://88.191.39.115/fabien/geotribu/%5bgeotribu%5d_Google-Maps_tuto13.html)


### Remarques




---


Toujours se référer à l'API Google Maps - [Google Maps API Reference](http://code.google.com/apis/maps/documentation/reference.html) pour les différentes classes, méthodes et options utilisées.
Les photos StreetView ne sont pas disponibles partout sur la Terre.


### Conclusion




---


Les méthodes relatives à StreetView commencent ici : <http://code.google.com/apis/maps/documentation/reference.html#GStreetviewPanorama>.


**Auteur : Fabien - fabien.goblet [ at ] gmail.com**




----

## Auteur

--8<-- "content/team/fgob.md"
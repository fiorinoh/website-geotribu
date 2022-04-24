---
authors:
- Fabien Goblet
categories:
- article
date: 2008-09-05 10:20
description: '...'
image: ''
license: default
robots: index, follow
tags:
- Google Maps
- KML
- infobulle
title: 8. Superposer un fichier KML et enrichir les infobulles
---

# 8. Superposer un fichier KML et enrichir les infobulles


:calendar: Date de publication initiale : 05 septembre 2008


----


![attention_light.jpg](/sites/default/files/Tuto/img/attention_light.jpg)**L'API Google Maps v2 est dépréciée depuis le 19 mai 2010.**


### Introduction




---


****L'API Google Maps permet de superposer des fichiers KML sur une carte. Et d'afficher une mini-carte dans une infobulle (ce qui n'a rien à voir avec l'affichage du KML mais qui peut être intéressant).****


### Initialisation




---


****Reprendre la carte du [deuxième tutoriel](http://geotribu.net/node/13).****


### Appel d'un fichier KML et affichage sur la carte




---


****L'appel à un fichier KML (dans le cas présent un fichier KML bien connu des [SIGMA](http://sigma.ensat.fr)-iens (-ieux, -iois, au choix)) se fait grâce à la classe GGeoXml :  `var kml = new GGeoXml("http://88.191.39.115/fabien/geotribu/kml/route.kml");`  Et l'affichage grâce à la méthode addOverlay() déjà utilisée précédemment :  `map.addOverlay(kml);`****


### Mini-carte dans une infobulle




---


****Créons tout d'abord un marqueur et ajoutons-le à la carte :  `var marker1 = new GMarker(new GLatLng(43.57769664771851, 1.402821992034912)); map.addOverlay(marker1);`  Puis gérons l'événement lors d'un clic sur celui-ci :  `GEvent.addListener(marker1, 'click', function() { marker1.showMapBlowup({zoomLevel:18,mapType:G_SATELLITE_MAP}); });`****


### Code complet




---


****`[Google Maps] 8. Superposer un fichier .kml et enrichir les info-bulles`****




### Démonstration




---






****[Résultat pleine page](http://88.191.39.115/fabien/geotribu/%5bgeotribu%5d_Google-Maps_tuto8.html)****


### Remarques




---


****Toujours se référer à l'API Google Maps - [Google Maps API Reference](http://code.google.com/apis/maps/documentation/reference.html) pour les différentes classes, méthodes et options utilisées. Le fichier XML doit toujours être une URL accessible. Consulter cette [page](http://code.google.com/apis/kml/documentation/kmlreference.html) pour les spécifications des fichiers KML.****


### Conclusion




---


**Ce tutoriel décrit les étapes pour ajouter un fichier KML à la carte et pour afficher une mini-carte dans une infobulle. Les possiblités d'utilisation de superposition d'un fichier sont nombreuses - exemple [le projet Inde à vélo](http://www.sigma2008.org/projects)  
**Auteur : Fabien - fabien.goblet [ at ] gmail.com****




----

## Auteur

--8<-- "content/team/fgob.md"
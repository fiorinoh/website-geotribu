---
authors:
- Fabien Goblet
categories:
- article
date: 2008-09-07 10:20
description: '...'
image: ''
license: default
robots: index, follow
tags:
- Google Maps
- marqueurs
- mysql
- Ajax
- PHP
title: 10. Utiliser Ajax, MySQL et PHP pour afficher des marqueurs
---

# 10. Utiliser Ajax, MySQL et PHP pour afficher des marqueurs


:calendar: Date de publication initiale : 07 septembre 2008


----





### Introduction




---


Ce tutoriel va nous permettre d'afficher des marqueurs stockés en base de données sur la carte Google Maps. Nous verrons donc comment ouvrir une liaison Ajax avec notre serveur, comment interpréter la réponse envoyée par le serveur et comment construire cette réponse en PHP.  



### Initialisation




---


Reprendre la carte du [tutoriel n°2](http://www.geotribu.net/node/13).  



### Définition




---


Déclarons tout d'abord une fonction qui crée un marqueur sur la carte et qui ouvre une infobulle lorqu'on clique dessus :  

`function createMarker(point,nom,url) {  

var marker = new GMarker(point);  

var html = "**["+nom+"](%5C%22%22+url+%22%5C%22)**";  

GEvent.addListener(marker, 'click', function() {  

marker.openInfoWindowHtml(html);  

});  

return marker;  

}`  



### Ajax




---


L'API Google Maps possède une méthode permettant une communication Ajax avec un script (ici le fichier ajax\_mysql.php) :  

`var urlstr = "./ajax_mysql.php";  

GDownloadUrl(urlstr, function(data) {  

var xml = GXml.parse(data);  

var markers = xml.documentElement.getElementsByTagName("marker");  

for (var i = 0; i < markers.length; i++) {  

var nom = markers[i].getAttribute("nom");  

var url = markers[i].getAttribute("url");  

var point = new GLatLng(parseFloat(markers[i].getAttribute("lat")),parseFloat(markers[i].getAttribute("long")));  

var marker = createMarker(point,nom,url);  

map.addOverlay(marker);  

}`  

Ici nous avons parsé le fichier XML envoyé par le script et nous avons appelé la fonction createMarker() pour chaque élément de type 'marker' du XML et en passant en paramètres le GPoint créé grâce aux attributs 'lat' et 'long', et les attributs 'nom' et 'url'.  



### Construction du XML côté serveur




---


Côté serveur, créer un fichier ajax\_mysql.php qui ouvre une connexion vers la base de données contenant les enregistrements et qui édite un fichier XML grâce aux fonctions du DOM XML :  

`php </p
$user = "******";  

$password = "*****";  

$host = "******";  

$bdd = "******";


mysql\_connect($host,$user,$password);  

mysql\_select\_db($bdd) or die("erreur de connexion à la base de données");  

$sql = "select * from test\_ajax\_mysql";  

$res = mysql\_query($sql) or die(mysql\_error());  

$dom = new DomDocument('1.0', 'iso-8859-1');  

$node = $dom->createElement("markers");  

$parnode = $dom->appendChild($node);  

while ($result = mysql\_fetch\_array($res)){  

$node = $dom->createElement("marker");  

$newnode = $parnode->appendChild($node);  

$newnode->setAttribute("nom", $result['nom']);  

$newnode->setAttribute("lat", $result['lat']);  

$newnode->setAttribute("long", $result['long']);  

$newnode->setAttribute("url", $result['url']);  

}  

$xmlfile = $dom->saveXML();  

echo $xmlfile;


?>`  



### Code complet




---


`-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">  







[Google Maps] 10. Utiliser Ajax, MySQL et PHP pour afficher des marqueurs  



html { overflow:hidden; height:100%; }
body { height:100%; margin:0; }
#map { width:100%; height:100%; }



function createMarker(point,nom,url) {
var marker = new GMarker(point);
var html = "<b><a href=\""+url+"\">"+nom+"</a></b>";
GEvent.addListener(marker, 'click', function() {
marker.openInfoWindowHtml(html);
});
return marker;
}

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


var urlstr = "./ajax\_mysql.php";
GDownloadUrl(urlstr, function(data) {
var xml = GXml.parse(data);
var markers = xml.documentElement.getElementsByTagName("marker");
for (var i = 0; i < markers.length; i++) {
var nom = markers[i].getAttribute("nom");
var url = markers[i].getAttribute("url");
var point = new GLatLng(parseFloat(markers[i].getAttribute("lat")),parseFloat(markers[i].getAttribute("long")));
var marker = createMarker(point,nom,url);
map.addOverlay(marker);
}
});

}
else{
alert('Désolé, mais votre navigateur n\'est pas compatible avec Google Maps');
}
}`  

`php </p
$user = "******";  

$password = "*****";  

$host = "******";  

$bdd = "******";


mysql\_connect($host,$user,$password);  

mysql\_select\_db($bdd) or die("erreur de connexion à la base de données");  

$sql = "select * from test\_ajax\_mysql";  

$res = mysql\_query($sql) or die(mysql\_error());  

$dom = new DomDocument('1.0', 'iso-8859-1');  

$node = $dom->createElement("markers");  

$parnode = $dom->appendChild($node);  

while ($result = mysql\_fetch\_array($res)){  

$node = $dom->createElement("marker");  

$newnode = $parnode->appendChild($node);  

$newnode->setAttribute("nom", $result['nom']);  

$newnode->setAttribute("lat", $result['lat']);  

$newnode->setAttribute("long", $result['long']);  

$newnode->setAttribute("url", $result['url']);  

}  

$xmlfile = $dom->saveXML();  

echo $xmlfile;


?>`  







[Résultat pleine page](http://88.191.39.115/fabien/geotribu/%5bgeotribu%5d_Google-Maps_tuto10.html)
### Démonstration




---




### Remarques




---


Toujours se référer à l'API Google Maps - [Google Maps API Reference](http://code.google.com/apis/maps/documentation/reference.html) pour les différentes classes, méthodes et options utilisées.
Cette méthode d'affichage de marqueur (on pourrait de la même manière afficher des polylignes ou des polygônes) n'est viable que pour un petit nombre d'enregistrements. En effet, à partir d'une centaine de points à afficher, cette méthode arrive à ses limites.
Dans ce cas, il devient obligatoire de 'fournir' des informations au script tels que l'extension géographique de la carte, d'utiliser une base de données avec une extension spatiale (par exemple [PostgreSQL](http://www.postgresql.org/) et [PostGIS](http://postgis.refractions.net/)) et / ou de 'clusteriser' les enregistrements (cf. [la carte des membres du forum Georezo](http://georezo.net/forum/map.php?sel=cv_user))


### Conclusion




---


Ce tutoriel décrit les étapes pour afficher des marqueurs via Ajax depuis un script PHP et une base de données MySQL.
Le manuel des fonctions DOM XML se trouve [ici](http://www.manuelphp.com/php/ref.domxml.php).
On peut imaginer une application de suivi de flotte de véhicules (ou de tortues marines ...) via cette méthode : les véhicules (ou les tortues) transmettant leurs coordonnées à la base de données, et de relancer la fonction GDownloadUrl() toutes les 30 secondes par exemple.


**Auteur : Fabien - fabien.goblet [ at ] gmail.com**




----

## Auteur

--8<-- "content/team/fgob.md"
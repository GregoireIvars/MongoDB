Exercice 1

Vous disposez du code JavaScript suivant qui comporte une fonction de conversion d’une distance exprimée en kilomètres vers des radians ainsi que d’un document dont les coordonnées serviront de centre à notre sphère de recherche. Écrivez la requête $geoWithin qui affichera le nom des salles situées dans un rayon de 60 kilomètres et qui programment du Blues et de la Soul.

```
var KilometresEnRadians = function(kilometres){ 
   var rayonTerrestreEnKm = 6371; 
   return kilometres / rayonTerrestreEnKm; 
}; 
 
var salle = db.salles.findOne({"adresse.ville": "Nîmes"}); 
 
var requete = { ... }; 
 
db.salles.find(requete ... }; 
```

  ### Réponse : 
```Javascript 
//Pour employé cette requete j'ai utiliser ma base de donnée Altlas me permettant de retourner les valeurs demandé . 
//J'emploie aussi en même temps les deux autre requetes donner juste au dessus .
//J'ai utiliser l'opérateur géospatial $geoWithin me permettant de rechercher tous les  documents dans ma collections qui se trouvent à l'intérieur de ma forme géométrique précisé. Dans notre cas, il s'agit d'une sphere. 
//L'utilisation de mon opérateur $centerSphere me permet de recherche tout les documents qui sont à l'intérieur de cette dernière.
//Elle ce centre sur un point et on définit son rayon avec le kilometresEnRadiant. 

//Ma requete : 
var KilometresEnRadians = function(kilometres){ 
   var rayonTerrestreEnKm = 6371; 
   return kilometres / rayonTerrestreEnKm; 
}; 
 
var salle = db.salles.findOne({"adresse.ville": "Nîmes"}); 
var requete = {
  "adresse.localisation": {
    $geoWithin: {
      $centerSphere: [
        [salle.adresse.localisation.coordinates[0], salle.adresse.localisation.coordinates[1]],
        KilometresEnRadians(60)
      ]
    }
  },
  "styles": {
    $in: ["blues", "soul"]
  }
} 

db.salles.find(requete, {"nom": 1})
```

Exercice 2: 

Écrivez la requête qui permet d’obtenir la ville des salles situées dans un rayon de 100 kilomètres autour de Marseille, triées de la plus proche à la plus lointaine :

``` 
var marseille = {"type": "Point", "coordinates": [43.300000, 5.400000]} 
 
db.salles.find(...) 
```

  ### Réponse : 
```Javascript 
//Cette requete est très similaire à la première effectuer sauf que on emplois les coordonnées par rapport à marseilles et on recherche dans un cercle de 100 km au tour de marseilles. 
//Et je viens ensuite trié le tout du plus proches au plus loin avec .sort
//Ma requete : 

var KilometresEnRadians = function(kilometres){ 
   var rayonTerrestreEnKm = 6371; 
   return kilometres / rayonTerrestreEnKm; 
}; 

var marseille = {"type": "Point", "coordinates": [43.300000, 5.400000]} 

var requete = {
  "adresse.localisation": {
    $geoWithin: {
      $centerSphere: [
        [marseille.coordinates[0], marseille.coordinates[1]],
        KilometresEnRadians(100)
      ]
    }
  },
}
db.salles.find(requete, {"nom": 1}).sort( { "adresse.localisation": 1 })
```

Exercice 3:

Soit polygone un objet GeoJSON de la forme suivante :

```
var polygone = { 
     "type": "Polygon", 
     "coordinates": [ 
            [ 
               [43.94899, 4.80908], 
               [43.95292, 4.80929], 
               [43.95174, 4.8056], 
               [43.94899, 4.80908] 
            ] 
     ] 
} 
```
  
Donnez le nom des salles qui résident à l’intérieur.

  ### Réponse : 

```Javascript

//N'arrivant pas à faire fonctionner ma requete avec le var polygone.
//J'ai entré directement les coordonnées dans ma requete. 
//J'ai alors employé l'opérateur spatial $polygon qui comme sont indique va venir rechercher les documents dont les coordonnées d'adresse se trouve à l'intérieur du polygone que j'ai définit. Soit par rapport au coordonnées précisé dans l'énoncé. 
//Je fini ensuite par un simple find m'affichant les nom des villes qui corresponds. 


//Ma requete : 
var requete = {
  "adresse.localisation": {
    $geoWithin: {
      $polygon:   [ 
               [43.94899, 4.80908], 
               [43.95292, 4.80929], 
               [43.95174, 4.8056], 
               [43.94899, 4.80908] 
            ]
    }
  },
}
db.salles.find(requete, {"nom": 1})

```

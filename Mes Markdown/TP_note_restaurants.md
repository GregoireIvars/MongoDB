Préparation des données:
a. Importez un jeu de données de localisation, comme les restaurants dans une ville. Vous pouvez utiliser la commande mongoimport pour importer des données dans MongoDB.
b. Assurez-vous d'avoir un champ de localisation géospatiale, comme la latitude et la longitude.

Requêtes MongoDB:
a. Recherchez les restaurants qui sont ouverts à partir de 18h00. Utilisez la méthode find () et les opérateurs de comparaison pour trouver les documents qui correspondent à vos critères.

```Javascript 
db.restaurant.find({"open_hours.openAft": {$eq: "18:00"}},{"restaurant_name": 1, "open_hours": 1})
```


b. Triez les restaurants par note, du plus haut au plus bas. Utilisez la méthode sort () pour trier les résultats.

 ```javascript 
//Ma requete
db.restaurant.find({"city": {$eq: "Lanslebourg Mont Cenis"}},{"avg_rating": 1, "restaurant_name": 1}).sort({"avg_rating": -1})
```


Indexation avec MongoDB:
a. Créez un index sur le champ d'ouverture des restaurants pour améliorer les performances de la recherche. Utilisez la méthode createIndex ().

```Javascript
db.restaurant.createIndex({"open_hours.openMorn": -1}, {"name": "openMorning_I"})
db.restaurant.createIndex({"open_hours.openAft": -1}, {"name": "openAfternoon_I"})
```
b. Vérifiez que l'index a été créé en utilisant la méthode listIndexes ().

```Javascript 
var getMyIndex = db.runCommand({listIndexes: "restaurant"}).cursor.firstBatch  
for (var i = 0; i < getMyIndex.length; i++){printjson(getMyIndex[i])}
```

Requêtes géospatiales:
a. Recherchez les restaurants qui se trouvent à moins de 2 km d'une certaine localisation. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.
```Javascript
 var restau = db.restaurant.findOne({"avg_rating": 3});
 var requete = {
   "location": {
     $nearSphere: {
       $geometry: {
        type : "Point",
         coordinates : [restau.localisation.coordinates[1], restau.localisation.coordinates[0]],
       },
       $minDistance: 2000,
       $maxDistance: 10000
     }
   },
 }
 db.restaurant.find(requete, {restaurant_name: 1})
```
b. Recherchez les restaurants qui se trouvent dans un certain rayon autour d'un point de localisation spécifique. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.
```javascript
var KilometresEnRadians = function(kilometres){ 
   var rayonTerrestreEnKm = 6371; 
   return kilometres / rayonTerrestreEnKm; 
}; 
 
var myPoint = {"type": "Point", "coordinates": [45.2883, 6.90027]} 
var requete = {
  "Localisations": {
    $geoWithin: {
      $centerSphere: [
        [myPoint.coordinates[0], myPoint.coordinates[1]],
        KilometresEnRadians(2)
      ]
    }
  },
} 
db.restaurant.find(requete, {restaurant_name: 1})
```

Framework d'agrégation:
a. Calculez la moyenne des notes des restaurants. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données. 
```Javascript
db.restaurant.aggregate([
   {
      $group: {
         _id: null,
         avgRating: { $avg: "$avg_rating" }
      }
   }
])
```

b. Trouvez les restaurants les plus populaires en fonction du nombre de commentaires. Utilisez le framework d'agrégation de MongoDB pour groupes les données et effectuer des calculs.

```Javascript 
var pipeline = [ 
  {  
      $unwind: "$avis.commmentaire"  
   },  
   {  
      $match: { "avis.commmentaire": {$exists: 1 }}  
   },  
   {  
      $group: {  
         _id: "$restaurant_name",  
         CommentaireListe: { $push: "$avis.commmentaire" }, Nombredecommentaire: {$count: {}}  
      }  
   },  
   {  
      $project: {  
         _id: 0,  
         nom: "$_id",  
         CommentaireListe: 1 , Nombredecommentaire: 1 
      }  
   },{$sort: {Nombredecommentaire: -1}}, {$limit: 10}  
]
db.restaurant.aggregate(pipeline)
```

Export de la base de données:
a. Exportez les résultats des requêtes dans un fichier CSV pour un usage ultérieur. Utilisez la commande mongoexport pour exporter des données de MongoDB.
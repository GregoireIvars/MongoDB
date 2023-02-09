Préparation des données:
a. Importez un jeu de données de localisation, comme les restaurants dans une ville. Vous pouvez utiliser la commande mongoimport pour importer des données dans MongoDB.
b. Assurez-vous d'avoir un champ de localisation géospatiale, comme la latitude et la longitude.

Requêtes MongoDB:
a. Recherchez les restaurants qui sont ouverts à partir de 18h00. Utilisez la méthode find () et les opérateurs de comparaison pour trouver les documents qui correspondent à vos critères.

```Javascript 
//j'utilise mon champs open_hours pour chercher quels restaurant sera ouvert à 18H00
//Ma requete :
db.restaurant.find({"open_hours.openAft": {$eq: "18:00"}},{"restaurant_name": 1, "open_hours": 1})
```


b. Triez les restaurants par note, du plus haut au plus bas. Utilisez la méthode sort () pour trier les résultats.

 ```javascript 
 //Mon jeu de donnée contenant une moyenne de note je l'ai employé pour définir e trié par note 
//Ma requete
db.restaurant.find({"city": {$eq: "Lanslebourg Mont Cenis"}},{"avg_rating": 1, "restaurant_name": 1}).sort({"avg_rating": -1})
```


Indexation avec MongoDB:
a. Créez un index sur le champ d'ouverture des restaurants pour améliorer les performances de la recherche. Utilisez la méthode createIndex ().

```Javascript
//Vu que un restaurant ouvre le matin et le soir j'ai préférer mettre une index sur les deux 
//Pour garantir une fluidité lors de requete concernant ces données. 
db.restaurant.createIndex({"open_hours.openMorn": -1}, {"name": "openMorning_I"})
db.restaurant.createIndex({"open_hours.openAft": -1}, {"name": "openAfternoon_I"})
```
b. Vérifiez que l'index a été créé en utilisant la méthode listIndexes ().

```Javascript 
//Une simple requete avec la méthode listIndexes me retourner une tableau composé d'élément nommé Object. 
//C'est object étant mes indexes j'ai trouvé une solution pour palier à mon problème : 
//Ma requete : 
var getMyIndex = db.runCommand({listIndexes: "restaurant"}).cursor.firstBatch  
for (var i = 0; i < getMyIndex.length; i++){printjson(getMyIndex[i])}
```

Requêtes géospatiales:
a. Recherchez les restaurants qui se trouvent à moins de 2 km d'une certaine localisation. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.
```Javascript
//Cette requete et la prochaine ne retourne aucune valeur. 
//Malgrès tout mes effort je n'ai pas reussi à comprendre pourquoi alors que ma requete concorde avec les données que je souhaite employé. 
//J'ai du créé un index géospatial sur mon champs localisation : 
db.restaurant.createIndex( { localisation : "2dsphere" } )
//Malgrès tout cette requete ne fonctionne pas nis la suivante d'ailleurs.
//L'opérateur $nearsphere ce base d'un point min et étend un cercle jusqu'a atteindre le point max. Venant retourner chaque restaurant présent dans ce cercle.
//Le point de départ est déterminer par le document ayant  l'_id.
//Ma requete : 
 var restau = db.restaurant.findOne({"_id": ObjectId('63e5127530ac02bdf30ec07d')});
 
 var requete = {
   "localisation": {
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
//La logique est présente dans la requete car théoriquement cette dernière répond à l'énoncé mais je ne comprend pas elle ne fonctionne pas 
//Ma requete : 
db.restaurant.find({
   location: {
      $nearSphere: {
         $geometry: {
            type: "Point",
            coordinates: [45.96, 1.16]
         },
         $maxDistance: 4000
      }
   }
},{"restaurant_name": 1})
```

Framework d'agrégation:
a. Calculez la moyenne des notes des restaurants. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données. 
```Javascript
//Ma requete permet de retourner la moyenne des note des restaurant .
//Comme déjà expliqué j'emploie l'objet "avg_rating" comme "note"
//Ma requete : 
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
//l'opérateur $unwind viens retier chaque élément de mon tableau. 
//Me permettant par la suite de pouvoir mieux les traiter et de les compter comme demander . 
//L'opérateur $match est la en guise de filtre de mes documents et c'est dans l'opérateur $group que je fais mes opération telle que le count. 
//Ma requete  : 
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



Voici l'ensemble des requêtes que j'ai du faire afin de rendre mon jeu de donnée exploitable. 

Me permettant répondre au exercice demander :  


J'ai d'abord du implémenter moi même la partie "open hours"
Je me suis basé sur le nom "city" pour répartir et créé quelque chose de fonctionnel.
```Javascript 
db.restaurant.updateMany({"city": {$eq: "Lanslebourg Mont Cenis"}}, {$set: {"open_hours": {"openMorn": "11:00", "closeMorn": "14:00", "openAft": "18:00","closeAft": "0:00"}}})

  

db.restaurant.updateMany({"city": {$eq: "Rocbaron"}}, {$set: {"open_hours": {"openMorn": "12:00", "closeMorn": "15:00", "openAft": "19:00","closeAft": "23:00"}}})

  

db.restaurant.updateMany({"city": {$eq: "Makkum"}}, {$set: {"open_hours": {"openMorn": "11:30", "closeMorn": "13:30", "openAft": "18:30","closeAft": "23:00"}}})

```


J'ai ensuite du ajouté un champ localisation me pouvant me permettre de gérer mes requête géospatial . 
Je me suis basé sur leur note afin de faire un semblant de trie pour répartir mes coordonnées par rapport à tout mes document.
```Javascript
db.restaurant.updateMany({"avg_rating": {$lte: 2}},
{$set: {"localisation": {"type": "Point", "coordinates": [45.285103,  6.877394]}}})

db.restaurant.updateMany({"avg_rating": {$gt: 4}},

{$set: {"localisation": {"type": "Point", "coordinates": [45.2883,  6.877394]}}})

db.restaurant.updateMany({"avg_rating": {$eq: 3}},

{$set: {"localisation": {"type": "Point", "coordinates": [45.24393, 6.939801]}}})
```


Pour finir j'ai créé des commentaires pour répondre à la dernière question de mon tp : 
J'ai encore employé par city pour répartir au mieux mes données. 
```Javascript 
db.restaurant.updateMany({"city": {$eq: "Makkum"}},

{$set: {"avis": {"commmentaire": ["What a commentaire", "this is a comment", "Great city", "voici un commentaire"]} }})

db.restaurant.updateMany({"city": {$eq: "Leerdam"}},

{$set: {"avis": {"commmentaire": ["Great city", "voici un commentaire"]} }})

db.restaurant.updateMany({"city": {$eq: "Lanslebourg Mont Cenis"}},

{$set: {"avis": {"commmentaire": ["this is a comment", "Great city", "voici un commentaire"]} }})

db.restaurant.updateMany({"city": {$eq: "Rocbaron"}},

{$set: {"avis": {"commmentaire": ["this is a comment", "Great city", "voici un commentaire"]} }})
```
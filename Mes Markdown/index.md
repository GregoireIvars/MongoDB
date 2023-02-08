Exercice 1

Un bref examen de vos fichiers journaux a révélé que la plupart des requêtes effectuées sur la collection salles cible des capacités ainsi que des départements, comme ceci :

```
db.salles.find({"capacite": {$gt: 500}, "adresse.codePostal": /^30/}) 
db.salles.find({"adresse.codePostal": /^30/, "capacite": {$lte: 400}}) 
```

Que proposez-vous comme index qui puisse couvrir ces requêtes ?

 ### Réponse : 
Afin de couvrir au mieux ces requêtes on viens indexer les deux champs grâce à : 
```javascript
db.salles.createIndex({"capacite" : -1, "adresse.codePostal": 1})
```
Je met en place l'indexation via une requête d'index composé permettant alors d'indexé deux champs d'un document en un requête .

Détruisez ensuite l’index créé.

 ### Réponse : 
Pour détruire mes index une fois créé, je récupère leur nom et fait une requête dropIndex.
```Javascript
db.salles.dropIndex("capacite_-1_adresse.codePostal_1")
```
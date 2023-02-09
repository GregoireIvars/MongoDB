## Exercice 1

*Écrivez le pipeline qui affichera dans un champ nommé ville le nom de celles abritant une salle de plus de 50 personnes ainsi qu’un booléen nommé grande qui sera positionné à la valeur « vrai » lorsque la salle dépasse une capacité de 1 000 personnes. Voici le squelette du code à utiliser dans le shell :*

 ### Réponse : 
Pour ce faire, j'ai employer  un **aggregate()** permettant : 
- de regroupez les valeurs de plusieurs documents.
- Effectuez  des opérations sur les données groupées pour obtenir un résultat unique 
- Analysez l'évolution des données  dans le temps 

Ce qui ma permis d(utiliser l'opérateur $match servant de filtrage 
et l'opérateur $project  qui permet de transmettre les document contenant les champs demandés à l'étape suivante du pipeline. Les champs spécifiés peuvent être  des champs existants dans les documents d'entrée ou des champs nouvellement calculés.   
``` javascript 

//Ma requete : 
var pipeline = [ {
		$match: {
		"capacite": {$gt: 50}
		}},{
		$project: {
		"_id": 0,
		"ville":"$nom",
		"grande": {$gt: ["$capacite", 1000]}
	},}] 
	db.salles.aggregate(pipeline) 
```


## Exercice 2

*Écrivez le pipeline qui affichera dans un champ nommé apres_extension la capacité d’une salle augmentée de 100 places, dans un champ nommé avant_extension sa capacité originelle, ainsi que son nom.*

 ### Réponse : 

```javascript
//Une requete employant les mêmes concept présenter çi-dessus. 
//J'ajoute juste une nouvelle opérateur d'aggregation qui est $add permettant d'ajouté à la valeur actuel du champs capacite un autre champs ou un entier directement
//Ma requete
var pipeline = [ {
		$project: {
		"_id": 0,
		"avant_extension": "$capacite",
		"apres_extension": {$add: ["$capacite", 100]}
	},}] 
	db.salles.aggregate(pipeline) 
```

## Exercice 3

*Écrivez le pipeline qui affichera, par numéro de département, la capacité totale des salles y résidant. Pour obtenir ce numéro, il vous faudra utiliser l’opérateur $substrBytes dont la syntaxe est la suivante :*

 ### Réponse : 
```javascript
//$substrBytes retourne une partie d'une chaîne de caractères, démarrant à partir du caractère situé à un index spécifié en octets UTF-8 (basé sur zéro) dans la chaîne, et comprenant un nombre défini d'octets.


//Ma requete 
var pipeline = [
{
		$project: {
		"_id": 0,
		"Numero_departement": {$substrBytes: ["$adresse.codePostal",0,2]}, 
		"CapaciteTotal": "$capacite",
	}},{ 
    $group: { 
        _id: "$Numero_departement",
    }}] 
	db.salles.aggregate(pipeline) 


```

## Exercice 4

*Écrivez le pipeline qui affichera, pour chaque style musical, le nombre de salles le programmant. Ces styles seront classés par ordre alphabétique.*

 ### Réponse : 

```javascript 
// l'opérateur d'aggregation $unwind  permet de décompressez un tableau dans chaque document pour créer un document distinct pour chaque élément du tableau. Nous permettant ensuite de traiter "styles" par "styles". 
//Je viens ensuite utiliser l'opérateur d'aggrégation $group qui nous permet de regrouper les documents par valeur par rapport au champs "styles" et viens compter le nombre d'occurences de chaque valeur 
//$sort permet de trie par ordre croissant d'identifiant.

//Ma requete : 
var pipeline = [  
{ $unwind: "$styles" },
{ 
    $group: { 
        _id: "$styles", 
        count: { $sum: 1 } } 
    }, 
    { 
        $sort: { _id: 1 } 
    }, 
    { 
        $project: { 
            styles: "$_id", 
            nombre_de_salles_programmant_ce: "$count", 
            _id: 0 
    } 
}
]
db.salles.aggregate(pipeline)
```

## Exercice 5

*À l’aide des buckets, comptez les salles en fonction de leur capacité :*

 ### Réponse : 

```Javascript

//L'operateur d'aggregation $bucket vien diviser les valeurs du champs "capacite" en catégories en se basant sur leurs valeurs. Toutes les valeurs supérieures ou inférieurs  à 1300 ou 100 seront regroupées dans la catégorie "Capacite supérieur ou inférieur a 1300".

//Ma requete : 
var pipeline = [
  {
    $group: {
      _id: "$capacite",
      count: { $sum: 1 }
    }
  },
  {
    $bucket: {
      groupBy: "$_id",
      boundaries: [100,300, 500, 700,900, 1000,1200,1300],
      default: "Capacite supérieur ou inférieur a 1300",
      output: {
        "nombre de salles": { $sum: 1 },
        capacité: { $push: "$_id" }
      }
    }
  }
];
db.salles.aggregate(pipeline);
```


*celles de 100 à 500 places*:

```Javascript

//Ma requete permettant d'obtenir les salles ayant une capacité entre 100 et 500 
var pipeline = [
  {
    $bucket: { 
            groupBy: "$capacite", 
            boundaries: [100, 500],
                default: "Nombres de salles ayant une capacité inférieur ou supérieur", 
            output: { 
                count: { $sum: 1 } 
      }
    }
  }
];
db.salles.aggregate(pipeline);
```


*celles de 500 à 5000 places* :

```Javascript 


//Ma requete permettant d'obtenir les salles ayant une capacité entre 500 et 5000 
var pipeline = [
  {
    $bucket: { 
            groupBy: "$capacite", 
            boundaries: [500, 5000],
                default: "Nombres de salles ayant une capacité inférieur ou supérieur", 
            output: { 
                count: { $sum: 1 } 
      }
    }
  }
];
db.salles.aggregate(pipeline);
```

## Exercice 6

*Écrivez le pipeline qui affichera le nom des salles ainsi qu’un tableau nommé avis_excellents qui contiendra uniquement les avis dont la note est de 10.*

 ### Réponse : 

```Javascript 

//JE réemplois toutes les opérateur d'aggregation que j'ai pu expliqué au préalable telle que : $unwind / $match / $group / $project.

//L'opérateur d'aggrégation $push permet de mettre la valeur du champs "avis" dans le tableau avis_excellents

//Ma requete : 
var pipeline = [ 
  {  
      $unwind: "$avis"  
   },  
   {  
      $match: { "avis.note": 10 }  
   },  
   {  
      $group: {  
         _id: "$nom",  
         avis_excellents: { $push: "$avis" }  
      }  
   },  
   {  
      $project: {  
         _id: 0,  
         nom: "$_id",  
         avis_excellents: 1  
      }  
   } 
]
db.salles.aggregate(pipeline)
```


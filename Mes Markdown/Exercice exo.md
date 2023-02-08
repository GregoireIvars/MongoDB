Voici la base de donnees qui permet d'effectuer la serie d'exercices : 

```Javascript 
db.salles.insertMany([ 
   { 
       "_id": 1, 
       "nom": "AJMI Jazz Club", 
       "adresse": { 
           "numero": 4, 
           "voie": "Rue des Escaliers Sainte-Anne", 
           "codePostal": "84000", 
           "ville": "Avignon", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.951616, 4.808657] 
           } 
       }, 
       "styles": ["jazz", "soul", "funk", "blues"], 
       "avis": [{ 
               "date": new Date('2019-11-01'), 
               "note": NumberInt(8) 
           }, 
           { 
               "date": new Date('2019-11-30'), 
               "note": NumberInt(9) 
           } 
       ], 
       "capacite": NumberInt(300), 
       "smac": true 
   }, { 
       "_id": 2, 
       "nom": "Paloma", 
       "adresse": { 
           "numero": 250, 
           "voie": "Chemin de l'Aérodrome", 
           "codePostal": "30000", 
           "ville": "Nîmes", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.856430, 4.405415] 
           } 
       }, 
       "avis": [{ 
               "date": new Date('2019-07-06'), 
               "note": NumberInt(10) 
           } 
       ], 
       "capacite": NumberInt(4000), 
       "smac": true 
   }, 
    { 
       "_id": 3, 
       "nom": "Sonograf", 
       "adresse": { 
           "voie": "D901", 
           "codePostal": "84250", 
           "ville": "Le Thor", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.923005, 5.020077] 
           } 
       }, 
       "capacite": NumberInt(200), 
       "styles": ["blues", "rock"] 
   } 
]) 
```

## Exercice 1
 ### Affichez l’identifiant et le nom des salles qui sont des SMAC.

 ### Réponse : 
Afin de pouvoir obtenir les identifiants et le nom des salles qui sont SMAC. 
J'ai employé la fonction **Find( )** permettant de gérer l'affichage des données présent dans les Documents de notre Collections.
```javascript 
db.collection.find(query, projection, options)
```

Comme présenter si dessus on comprend que notre requêtes va avoir besoin d'un paramètre query et d'un paramètre projection afin de d'obtenir les documents ayant SMAC a true et afficher seulement le nom et l'id de la salle .
Pour ce faire j'ai utiliser : 
```Javascript 
db.salles.find({"smac" : {$eq : true}}, {"_id": 1, "nom": 1})

// Cette requête me retourne : 
{ 
_id: 1,
nom: 'AJMI Jazz Club'
}
{  
_id: 2,
nom: 'Paloma'
m}

```

## Exercice 2 : 

 ### Affichez le nom des salles qui possèdent une capacité d’accueil strictement supérieure à 1000 places.
 
 ### Réponse : 

```Javascript 
//Je reprend la fonction find() 
db.salles.find({"capacite": {$gt: 1000}}, {"_id": 0, "nom": 1 , "capacite": 1})
//j'emploie l'opérateur $gt qui signifie strictement supérieur 
//j'ai aussi ajouté une projection pour rendre mon résultat de requête plus propre.

//Résultat de cette requête : 
{
  nom: 'Paloma',
  capacite: 4000
}
```

## Exercice 3

 ### Affichez l’identifiant des salles pour lesquelles le champ adresse ne comporte pas de numéro.
 
 ### Réponse : 
```javascript 

//Ma requête : 
db.salles.find({"adresse.numero": null},{"_id": 1})

//Résultat de ma requête : 
{
  _id: 3
}

```
 

## Exercice 4

 ### Affichez l’identifiant puis le nom des salles qui ont exactement un avis.
 
 ### Réponse : 
 
Notre but est  d'obtenir les dimensions de notre champs avis. Sachant que ce dernier est un tableau, l'opérateur **$size** peut être utiliser. On peut alors obtenir le résultat attendu.
 ```Javascript
 //Ma requête : 
 db.salles.find({"avis": {$size: 1}},{"_id": 1, "nom": 1})

//Résultat de ma requête : 
{
  _id: 2,
  nom: 'Paloma'
}

```
 


## Exercice 5

 ### Affichez tous les styles musicaux des salles qui programment notamment du blues.
 
 ### Réponse :
```Javascript 
//Ma requête 
db.salles.find({"styles": "blues"}, {"_id": 0 , "styles": 1, "nom": 1})

//Résultat de ma requete :
{
	nom: 'AJMI Jazz Club',
	styles: [
		'jazz',
		'soul',
		'funk',
		'blues',
	]
},
{
	nom: 'Sonograf',
	styles: [
		'blues',
		'rock',
	]
}

```

## Exercice 6

 ### Affichez tous les styles musicaux des salles qui ont le style « blues » en première position dans leur tableau styles.

 ### Réponse :
 ```Javascript 
 //J'ai employer styles.0 pour qu'il me retourne la valeur rentré en première position.
 //Ma requete : 
 db.salles.find({"styles.0": "blues"},{"nom": 1, "styles": 1})

//Résultat de ma requete : 
{
	_id: 3,
	nom: 'Sonograf',
	styles: [
		'blues',
		'rock',
	]
}

```


## Exercice 7

 ### Affichez la ville des salles dont le code postal commence par 84 et qui ont une capacité strictement inférieure à 500 places (pensez à utiliser une expression régulière).
 
  ### Réponse :
  ```Javascript 
//J'emploie une expression régulière pour obtenir tout les documents ayant un code postal commençant par 84. J'ai aussi employé l'opérateur $lt signifiant strictement inférieur. Ensuite, j'ai simplement remplis le parametre projection avec ce que je souhaité ou non afficher. 
//Ma requete : 
  db.salles.find({"adresse.codePostal": {$regex: /^84/i },"capacite": {$lt: 500}}, {"_id": 0, "nom": 1, "adresse.ville": 1, "adresse.codePostal": 1, })

//Résultat de ma requete: 
{
	nom: 'AJMI Jazz Club',
	adresse: {
		codePostal: '84000',
		ville: 'Avignon'
	}
},
{
	nom: 'Sonograf',
	adresse: {
		codePostal: '84250',
		ville: 'Le Thor'
	}
}

```
  

## Exercice 8

 ### Affichez l’identifiant pour les salles dont l’identifiant est pair ou le champ avis est absent.

  ### Réponse :
  ```Javascript 
//Souhaitant obtenir tout les documents ayant un id pair j'ai employé l'opérateur $mod (modulo) et j'ai simplement divisé par 2 et attendu comme reste 0. 
// J'ai aussi employé l'opérateur $or (où) car le  résultat demandé est soit un document avec un id pair soit avec le champs avis absent. 

//Ma requete:
db.salles.find({$or:[{"_id": {$mod: [2, 0]}}, {"avis": null}]}, {"_id": 1, "nom": 1, "avis": 1})

//Résultat de ma requete :

{
	_id: '2',
	nom: 'Paloma',
	avis: [{
		date: 2019-07-06T00:00:00.000+00:00Z,
		note: 10
		}
	]
},
{
	_id: '3',
	nom: 'Sonograf',
}

```


## Exercice 9

 ### Affichez le nom des salles dont au moins un des avis comporte une note comprise entre 8 et 10 (tous deux inclus).

  ### Réponse :
```Javascript 
//J'ai employé les opérateurs $gte et $lte signifiant supérieur ou égale et inférieur ou égale me permettant alors d'obtenir les documents ayant un avis avec une note comprise entre 8 et 10 (inclus).

//Ma requete : 
db.salles.find({"avis.note": {$gte: 8, $lte: 10}}, {"nom": 1})

//Résultat de ma requete: 
{
	_id: '1',
	nom: 'AJMI Jazz Club',
}
{
	_id: '2',
	nom: 'Paloma',
}
```

## Exercice 10

 ### Affichez le nom des salles dont au moins un des avis comporte une date postérieure au 15/11/2019 (pensez à utiliser le type JavaScript Date).

  ### Réponse :
```Javascript 
//J'ai employer le type javascript Date me permettant alors de comparer les champs date avec la valeur Date que j'ai mis. 

//Ma requete : 
db.salles.find({"avis.date": {$lt: new Date("2019-11-15T00:00:00.000+00:00")}}, {"nom": 1, "avis": 1})

//Résultat de ma requete: 
{
    _id: 1,
        nom: 'AJMI Jazz Club',
            avis: [{
                date: 2019 - 11 - 01T00: 00: 00.000Z,
                note: 8
            }, {
                date: 2019 - 11 - 30T00: 00: 00.000Z,
                note: 9
            }
            ]
}
{
    _id: 2,
        nom: 'Paloma',
            avis: [{
                date: 2019 - 07 - 06T00: 00: 00.000Z,
                note: 10
            }]
}
```


## Exercice 11

 ### Affichez le nom ainsi que la capacité des salles dont le produit de la valeur de l’identifiant par 100 est strictement supérieur à la capacité.

  ### Réponse :
```Javascript 

//L'operateur $exists comme sont nom l'indique précise si le champs nom existe dans mes documents de ma collections. 
//J'ai employé l'operateur $expr qui permet l'utilisation d'expressions d'agrégation dans le langage de requête.
//L'opérateur $multiply est la pour faire la multiplication entre la valeur de mon champs _id et 100 et vien ensuite comparer le résultat à capacité.

//Ma requete : 
db.salles.find({
	"nom": {$exists: 1},
	"capacite": {$exists: 1},
	$expr: {$gt: [{$multiply: ["$_id",100]}, "$capacite"]}
},{
 "nom": 1,
 "_id": 1, "capacite": 1,
})


//Résultat de ma requete : 
db.salles.find({
    "nom": {$exists: 1},
    "capacite": {$exists: 1},
    $expr: {$gt: [{$multiply: ["$_id",100]}, "$capacite"]}
},{
 "nom": 1,
 "_id": 1, "capacite": 1,
})
{
  _id: 3,
  nom: 'Sonograf',
  capacite: 200
}

```

## Exercice 12

 ### Affichez le nom des salles de type SMAC programmant plus de deux styles de musiques différents en utilisant l’opérateur $where qui permet de faire usage de JavaScript.

  ### Réponse :

```Javascript 
//L'opérateur $where nous permet d'employe du javascript pur et dure.
//Mais reste malgrès tout peu recommandable dans un gros projet. 

//Ma requete : 
db.salles.find({$where: "this.smac == true && this.styles?.length >2"}, {"nom": 1})

//Résultat de ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club'
}
```

## Exercice 13

 ### Affichez les différents codes postaux présents dans les documents de la collection salles.

  ### Réponse :
```Javascript
//Requete prenant en compte le tableau adresse comportant des objets telle que codePostal. 
//Nous permettant alors de retourner les différents codes postaux présents dans les documents de la collection salles. 

//Ma requete : 
db.salles.find({"adresse.codePostal": {$exists: 1}},{"adresse.codePostal": 1})

//Résultat de ma requete : 
{
  _id: 1,
  adresse: {
    codePostal: '84000'
  }
}
{
  _id: 2,
  adresse: {
    codePostal: '30000'
  }
}
{
  _id: 3,
  adresse: {
    codePostal: '84250'
  }
}
```

## Exercice 14

 ### Mettez à jour tous les documents de la collection salles en rajoutant 100 personnes à leur capacité actuelle.

  ### Réponse :
```Javascript
//J'emploie la fonction **updateMany** permettant de modifier plusieurs documents en une requete .
//Je viens vérifier si dans chaque documents un champs _id existe et ensuite je leurs incrémente via l'opérateur $inc 100 personnes à leur capacité. 

//Ma requete : 
db.salles.updateMany({"_id": {$exists: 1}},{$inc: {"capacite": 100}})

//Mes Documents avant ma requete : 

{
  _id: 1,
  nom: 'AJMI Jazz Club',
  capacite: 300
}
{
  _id: 2,
  nom: 'Paloma',
  capacite: 4000
}
{
  _id: 3,
  nom: 'Sonograf',
  capacite: 200
}

//Mes Documents après ma requete : 

{
  _id: 1,
  nom: 'AJMI Jazz Club',
  capacite: 400
}
{
  _id: 2,
  nom: 'Paloma',
  capacite: 4100
}
{
  _id: 3,
  nom: 'Sonograf',
  capacite: 300
}
```

## Exercice 15

 ### Ajoutez le style « jazz » à toutes les salles qui n’en programment pas.

  ### Réponse :
```Javascript
//On réemploie la fonction updateMany pour modifier plusieur documents en 1 fois . 
//Et on ajoute le style "jazz" à toutes les salles qui n'en programment pas 

//Ma requete : 
db.salles.updateMany({"styles": {$ne: "jazz"}},{$push: {"styles": "jazz"}})
//On peut aussi utiliser une requete différente et obtenir le même résultat : 
db.salles.updateMany({"_id": {$exists: 1}},{$addToSet: {"styles": "jazz"}})
//Dans les deux cas on empêche la possibilité de doublon. 

//Mes Documents avant ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  styles: [
    'jazz',
    'soul',
    'funk',
    'blues'
  ]
}
{
  _id: 2,
  nom: 'Paloma',
}
{
  _id: 3,
  nom: 'Sonograf',
  styles: [
    'blues',
    'rock'
  ]
}

//Mes Documents après ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  styles: [
    'jazz',
    'soul',
    'funk',
    'blues'
  ]
}
{
  _id: 2,
  nom: 'Paloma',
  styles: [
    'jazz'
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  styles: [
    'blues',
    'rock',
    'jazz'
  ]
}
```


## Exercice 16

 ### Retirez le style «funk» à toutes les salles dont l’identifiant n’est égal ni à 2, ni à 3.

  ### Réponse :
```Javascript 
//L'opérateur $nin permet de définir que on ne veut pas un id egale à 2 ni à  3 
//Et l'opérateur pull permet quand à lui de retirer l'élément "funk" du champs styles des documents en question. 

//Ma requete : 
db.salles.updateMany({"_id": {$nin: [2,3]}, "styles": {$exists: 1}},{$pull: {"styles": "funk"}})
//Mes Documents avant ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  styles: [
    'jazz',
    'soul',
    'funk',
    'blues'
  ]
}
{
  _id: 2,
  nom: 'Paloma',
  styles: [
    'jazz'
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  styles: [
    'blues',
    'rock',
    'jazz'
  ]
}

//Mes Documents après ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  styles: [
    'jazz',
    'soul',
    'blues'
  ]
}
{
  _id: 2,
  nom: 'Paloma',
  styles: [
    'jazz'
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  styles: [
    'blues',
    'rock',
    'jazz'
  ]
}
```

## Exercice 17

 ### Ajoutez un tableau composé des styles «techno» et « reggae » à la salle dont l’identifiant est 3.

  ### Réponse :
```Javascript
//L'opérateur $in viens vérifier si l'id présent est 3 .
//On vien ensuite ajouté notre tableau de styles avec $addToSet permettant d'évité l'ajout de doublon . L'opérateur push peut être aussi utilisé mais pour ce faire faut ajouté une query vérifiant que les valeurs mise n'existe pas ($ne).

//Ma requete : 
db.salles.updateMany({"_id": {$in: [3]}, "styles": {$exists: 1}},{$addToSet: {"styles": ["techno", "reggae"]}})

//Mes Documents avant ma requete : 

{
  _id: 1,
  nom: 'AJMI Jazz Club',
  styles: [
    'jazz',
    'soul',
    'blues'
  ]
}
{
  _id: 2,
  nom: 'Paloma',
  styles: [
    'jazz'
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  styles: [
    'blues',
    'rock',
    'jazz'
  ]
}

//Mes Documents après ma requete : 

{
  _id: 1,
  nom: 'AJMI Jazz Club',
  styles: [
    'jazz',
    'soul',
    'blues'
  ]
}
{
  _id: 2,
  nom: 'Paloma',
  styles: [
    'jazz'
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  styles: [
    'blues',
    'rock',
    'jazz'
     [
      'techno',
      'reggae'
    ]
  ]
}


```

## Exercice 18

 ### Pour les salles dont le nom commence par la lettre P (majuscule ou minuscule), augmentez la capacité de 150 places et rajoutez un champ de type tableau nommé contact dans lequel se trouvera un document comportant un champ nommé telephone dont la valeur sera « 04 11 94 00 10 ».

  ### Réponse :
```Javascript 
//Ma regex permet la vérification que le champs nom de mon documents commence bien par p ou P 
//J'incrémente ensuite la capacite avec l'opérateur $inc.
//Et j'utilise $set pour venir créé et ajouté un nouveau champs contenant un talbeau d'objet  dans le documents en question 

//Ma requete : 
db.salles.updateMany(
{
  "nom": {$regex: "^[pP]"},
  "capacite": {$exists: 1}
},{$inc: {"capacite": 150}, $set: {"contact": [{"telephone": "04 11 94 00 10"}]}})

//Mes Documents avant ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  capacite: 400
}
{
  _id: 2,
  nom: 'Paloma',
  capacite: 4100
}
{
  _id: 3,
  nom: 'Sonograf',
  capacite: 300
}

//Mes Documents après ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  capacite: 400
}
{
  _id: 2,
  nom: 'Paloma',
  capacite: 4250,
  contact: [
    {
      telephone: '04 11 94 00 10'
    }
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  capacite: 300
}
```

## Exercice 19

 ### Pour les salles dont le nom commence par une voyelle (peu importe la casse, là aussi), rajoutez dans le tableau avis un document composé du champ date valant la date courante et du champ note valant 10 (double ou entier). L’expression régulière pour chercher une chaîne de caractères débutant par une voyelle suivie de n’importe quoi d’autre est [^aeiou]+$.


  ### Réponse :
```Javascript 
//Ma requete : 
db.salles.updateMany(
{
 "nom": {$regex: "^[aeiou]+$"}
},{
$addToSet: {"avis": [{"date": new Date(), "note": 10}]}
})

//Mes Documents avant ma requete : 
{
    _id: 1,
    nom: 'AJMI Jazz Club',
    avis: [{
         date: 2019-11 -01T00:00:00.000Z,
         note: 8
 }, {
         date: 2019-11-30T00:00:00.000Z,
         note: 9
 }]
}
{
	_id: '2',
	nom: 'Paloma',
	avis: [{
		date: 2019-07-06T00:00:00.000+00:00Z,
		note: 10
		}
	]
},
{
	_id: '3',
	nom: 'Sonograf',
},


//Mes Documents après ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club',
  avis: [
    {
      date: 2019-11-01T00:00:00.000Z,
      note: 8
    },
    {
      date: 2019-11-30T00:00:00.000Z,
      note: 9
    },
    [
      {
        date: 2023-02-07T21:35:34.320Z,
        note: 10
      }
    ]
  ]
}
{
  _id: 2,
  nom: 'Paloma',
  avis: [
    {
      date: 2019-07-06T00:00:00.000Z,
      note: 10
    }
  ]
}
{
  _id: 3,
  nom: 'Sonograf',
  avis: [
    [
      {
        date: 2023-02-07T21:35:34.320Z,
        note: 10
      }
    ]
  ]
}
```

## Exercice 20

 ### En mode upsert, vous mettrez à jour tous les documents dont le nom commence par un z ou un Z en leur affectant comme nom « Pub Z », comme valeur du champ capacite 50 personnes (type entier et non décimal) et en positionnant le champ booléen smac à la valeur « false ».

  ### Réponse :
```Javascript
//Ma requete vien créé un nouveau document qui à comme nom pub Z un smac a false et une capacite de 50 et je viens aussi lui générer via upsert un id de type ObjectId.

//Ma requete : 
db.salles.updateMany({"nom": {$regex: "^[Zz]"}},{$set: {"nom": "Pub Z","smac": false, "capacite": 50}},{"upsert": true })

//Mes Documents avant ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club'
}
{
  _id: 2,
  nom: 'Paloma'
}
{
  _id: 3,
  nom: 'Sonograf'
}

//Mes Documents après ma requete : 
{
  _id: 1,
  nom: 'AJMI Jazz Club'
}
{
  _id: 2,
  nom: 'Paloma'
}
{
  _id: 3,
  nom: 'Sonograf'
}
{
  _id: ObjectId("63e2c5e02cf1f8f571302997"),
  nom: 'Pub Z'
}

```



## Exercice 21

 ### Affichez le décompte des documents pour lesquels le champ _id est de type « objectId ».

  ### Réponse :
```Javascript
//Requete venant compter le nombre de document qui possèdes un _id de type objectId dans ma collections.
//Connaissant mes collections de documents on sais que le nombre attendu est 1 

//Ma requete : 
db.salles.find({"_id": {$type: ["objectId"]}}).count()

//Résultat de ma requete : 
1 
```


## Exercice 22

 ### Pour les documents dont le champ _id n’est pas de type « objectId », affichez le nom de la salle ayant la plus grande capacité. Pour y parvenir, vous effectuerez un tri dans l’ordre qui convient tout en limitant le nombre de documents affichés pour ne retourner que celui qui comporte la capacité maximale.

  ### Réponse :
```Javascript
// Cette requete emploie l'opérateur $not pour vérifier lequels de mes documents ne contionnent pas de champs _id de type objectId.
//On viens ensuite trie par capacité decroissant et on le limite à 1 affichage pour obtenir que le documents ayant la plus grande capacité. 

//Ma requete : 
db.salles.find({"_id": {$not: {$type: ["objectId"]}}},{"nom": 1, "capacite": 1}).sort({"capacite":-1}).limit(1)

//Résultat de ma requete : 
{
  _id: 2,
  nom: 'Paloma',
  capacite: 4250
}

```
## Exercice 23

 ### Remplacez, sur la base de la valeur de son champ _id, le document créé à l’exercice 20 par un document contenant seulement le nom préexistant et la capacité, que vous monterez à 60 personnes.

  ### Réponse :
```Javascript 
//Ma requete ce base sur le document créé durant l'exercice 20 grâce à l'id.
//on viens alors retirer via $unset le champs smac de mon document et passer la capacité de ce derniers à 60

//Ma requete : 
db.salles.updateMany({"_id": {$type: ["objectId"]}},{$set: {"capacite": 60},$unset: {smac: ""}})

//Résultat de ma requete : 
{
  _id: ObjectId("63e2c5e02cf1f8f571302997"),
  capacite: 60,
  nom: 'Pub Z'
}

```


## Exercice 24

 ### Effectuez la suppression d’un seul document avec les critères suivants : le champ _id est de type « objectId » et la capacité de la salle est inférieure ou égale à 60 personnes.

  ### Réponse :
```Javascript 
//La fonction deleteOne permet de venir supprimé un document de notre collection.
//Ma requete : 
db.salles.deleteOne({"_id": ObjectId("63e26684e3fba2fe2f04e88c"), "capacite": {$lte: 60}})
```



## Exercice 25

 ### À l’aide de la méthode permettant de trouver un seul document et de le mettre à jour en même temps, réduisez de 15 personnes la capacité de la salle située à Nîmes.

  ### Réponse :
```Javascript
//Ma requete : 
db.salles.updateOne({"adresse.ville": "Nîmes"},{$inc: {"capacite": -15}})
//Mes Documents avant ma requete :
{
  _id: 2,
  nom: 'Paloma',
  adresse: {
    ville: 'Nîmes'
  },
  capacite: 4250
}
//Mes Documents après ma requete : 
{
  _id: 2,
  nom: 'Paloma',
  adresse: {
    ville: 'Nîmes'
  },
  capacite: 4235
}
```


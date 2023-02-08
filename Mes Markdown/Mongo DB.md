
## Introduction : 

#### Qu'est ce que c'est MONGO DB ? 

SGBD orienté document et complétement cross-plateform. 
On peut y accéder via le cloud
Mongo DB est un système de gestion de base de donnée qui viens stocker les dossier sous format JSON . 
Aucune structure n'est imposé 
Les documents permettent de regrouper des informations. 

#### Qu'est ce qu'une base de donnée (Database) 

La base de données est le conteneur physique des collections. (espace disque)
Chaque base de données possède ses propres fichiers dans le système de gestion de fichier sur la machine hôte


#### Collections  

Une collections est  un groupe de documents MongoDB. C'est l'équivalent d'une table dans un système de  gestion de base de données. 
Les collections n'imposent pas de schéma précis. 
Les documents au sein d'une même collection peuvent avoir des champs différents. 
Malgré tout les différent document reste pour le moins très similaire. 

#### Document 

Un document est un ensemble de clé valeur (Objet). 
Le schéma des document est dit "dynamique".


#### Résumer  

Une **Database** est composé de plusieurs **Collections** et les collections quand à eux sont composé de différent **Document**.


#### Comparaison avec SQL 

L'équivalent des **table** corresponde à une **collection** . 
**Des lignes**  = **Document** 
**Colonne**  = **field** 
**Jointure** = **Document imbriqués**

#### Avantage de Mongo DB 

Pas de schéma / Structure claire et simple : objet / pas de jointure complexe / 
Des requêtes imbriqués et dynamique (presque aussi performant que SQL) / 
Très facile de "squale " / Stockage sur disque / plus besoin d'ORM 

Mongo DB fait tout bien mais il ne sera jamais n°1 



#### Mongo DB

##### JSON  / BSON :

JSON est un format de données textuelles dérivé de la notation des objets du langages Javascript. 
Il est facile à lire et écrire pour l'humain.
Aisément analysable.

JSON se base sur deux structures : 
 -   Une collection de couples nom/valeur. Divers langages la réifient par un _objet_, un enregistrement, une structure, un dictionnaire, une table de hachage, une liste typée ou un tableau associatif.
-   Une liste de valeurs ordonnées. La plupart des langages la réifient par un _tableau_, un vecteur, une liste ou une suite.

BSON -> JSON 
JSON -> BSON 

Un document **BSON** peut être de taille 16Mo.
**BSON** est un codage binaire de la notation d'objet Javascript (JSON) 
une notation d'objet textuelle largement utilisée pour transmettre et stocker des données dans des applications web. 
**JSON** est plus facile à comprendre car il est lisible par l'homme, mais comparé à BSON, il supporte moins de types de données.
**BSON** encode également les informations de type et de longueur, ce qui facilite l'analyse par les machines.

- `{}` Ceci représente un Objet vide en JSON

Les propriété d'un objet JSON sont matérialisée par  un ou plusieurs paires cle/valeur :

```JSON 
{
"clé": "valeur", 
"uneAutreCle": {"autreclé": "uneautreValeur"},
"untableau": []
}
```

 ### JSON prend en charge de façon native  plusieurs formes de données : 

- Boolean
- Numérique
- Chaine de caractère 
- Tableau 
- Objet 
- Null (indique l'absence de valeur)

 ### Mongo DB viens à sont tour rajouté de types : 

- Le type **Date** stocke sous ma forme d'un entier signe de 8 octets
Pour que une durée devienne une date, il nous faut un point départ. 

**EPOCH** (1 janvier 1970) est  une convention donc on se base sur cette date.
Représente le temps écouler depuis le temps UNIX

**UNIX**  est une famille de systèmes d'exploitation multitâche et multi-utilisateur dérivé du Unix d'origine créé par AT&T, le développement de ce dernier ayant commencé dans les années 1970.
Comme Linux / GNU / BSD.

-  Le type **ObjetId** stocke 12 octets, utilise en interne / Mongo DB afin de gérer les identifiants.

-  Le type **NumberLong** et le type **NumberInt** :  par default Mongo DB considérer toutes valeur numérique comme un nombre à virgule code sur 8 octets.
   Représentent des entiers signes sur 8 et 4 octets.

-  Le type **BinData** : pour stocker des chaînes de caractères ne possédant pas de représentation dans l'encodage **UTF-8** 
 **UTF8** (_UCS transformation format_ 8 bits) est un format de codage de caractères. Il permet de gérer tous les caractères dits unicodes.
 Chaque caractère est codé sur un ou plusieurs points de code.


### Mongo DB en ligne de commande : 

#### Lancement de Mongo DB : 

```Bash 

mongod

||

mongod --port 27017

||

mongod --port 27017 --dbpath /data/databases --logpath /tmp/mongodb.log --logappend


```


#### Commande sur Compass : 

```mongoShell

show database  || show collections 

db  (affiche la base de donnée)

use + ... (permet de changer de base de donnée)

db.createUser ({"user": "Root", "pwd": "root", "roles": [{"userAdmin", "db": "admin"}, "readWriteAnyDatabase"]}) (Créé un utilisateur et lui confie certain droit)




```


Pour créé une base de donnée sur Mongo DB il n'existe pas une commande implicite venant nous la générer. 
C'est en utilisant **use + nom** de la **Base** et une fois que on aura ajouté une **Collection** à l'intérieur  avec un **Document** dans ce dernier 
Alors notre BDD sera alors créer et affiché.

Pour crée votre DB : 
```Javascript
use + Le nom de votre bdd

db.uneCollection.insert({"uneCle": "uneValeur"}) (Crée notre collections )
```


Pour supprimé votre DB  : 
 ```Javascript
use + nom de votre bdd a supprimer

db.dropDatabase()
```

Affiche les éléments de votre collections de votre BDD : 
```Javascript
use + nom de votre BDD

db.uneCollection.find() (Permer dafficher les Documents de ma Collections)
(Equivaut au Select *)

```

Afficher les détails d'une collections : 

```Javascript

db.runCommand({"collstats": "uneCollection"})
```


### Gestion des collections : 

Collations sert à définir des règles sur les chaines de caractère.

```Javascript
{
	**locale**: <string>,
	caseLevel: <boolean>,
	caseFirst: <string>,
	strength: <entier>,
	numeriOrdering: <boolean>,
	...
}
```

Au sein de ce type de document le champ locale est obligatoire.


On est pas dans l'obligation pour créer une collection de recréer une BDD.
On peut passer directement à la création une collection. 

```Javascript
Créé une collection : 
db.createCollection("newCollection", {"collation": {"locale": "fr"}})

Supprimer une collection : 
db.newCollection.drop()

Renommer une collection : 
db.ewCollection.renameCollection("newCollection")
```



### Les documents

Une collections possèdes diverse documents.
Lors de notre création de notre base de donnée accompagné de sa collection.
On remarque que on créé aussi un document par la même occasion. 

```Javascript
//Insert un Document dans la Collection "uneCollection" : 

db.uneCollection.insert({"uneCle": "uneValeur"})

//insersion d'une collection personnes possédant deux document distinct : 

db.personnes.insert([
{"Nom": "leNom", "Prénom": "lePrénom"},
{"Nom": "Test", "Prénom": "BOB", "age": 20}
])


```


Mise  à part créer de nouveau document on peut également les mettre à jour avec une Update : 
```Javascript
db.collection.updateOne(<filtre>, <modification>)

Change le Prénom du document ayant comme Nom "leNom" : 

db.personnes.update({"Nom": "leNom"},{
$set: {"Prénom": "Toto"}
})

```

Validation de Document : 

```Javascript 
var athletes = [{
"nom": "Eclair",
"prenom": "Jean-Michel",
"discipline": "Course"
},{
"nom": "Allen",
"prenom": "Barry",
"discipline": "Course"
},{
"nom": "Cena",
"prenom": "John",
"discipline": "Catcher"
}   
]

 
```

Exemple de validation de document : 

```Javascript
var proprietes = [{
"nom": {
"bsonType": "string",
"pattern": "^[A-Z].*",
"description": "Chaine de caractere + RegEXP - obligatoire"},
"prenom":{
"bsonType": "string",
"description": "Chaine de caractere  - obligatoire"},
"discipline": {
"enum": ["Course", "Catcher"],
"description": "Liste de sport possibles - obligatoire"},
}]
```

Une fois nos règles établies, on va modifier notre collection a l'aide d'un commande : 

```Javascript

db.runCommand(
{
	"collMod": "athletes",
	"validator": {
		$jsonSchema: {
			"bsonType": "object",
			"required": ["nom", "prenom", "discipline"],
			"properties": "proprieties"
		}
	}
}
)
```


#### Retourner des données spécifique 

Quand on des collections composé d'un nombre important de documents.
Pour ce faire une peut employer une command Find nous permettant de retourner les données demander selon nos conditions spécifier : 

```Javascript 
db.collection.find({"age": {$eq: 76}})

//Retourne tout les document de notre collection possédant un age égale à 76
```

D'autre opérateur sont à notre disposition : 


On peut egalement combiner  ces operateur pour effectuer des recherches sur des intervalles : 

```javascript 
db.collection.find({"age": {$lt: 50, $gt: 20}})
Retourne tout document de notre collections possédant un age compris entre 50 et 20 
db.collection.find({"age": {$exists: true}})
Retourne tout document possédant le champ âge.
```

#### Agrégation 

Les opérations d'agrégation traitent plusieurs documents et renvoient des résultats calculés.
Vous pouvez utiliser les opérations d'agrégation pour :
- Regrouper les valeurs de plusieurs documents.
- Effectuer des opérations sur les données groupées pour renvoyer un résultat unique.
-  Analyser l'évolution des données dans le temps.

Exemple d'agrégation : 
 ```Javascript 
 db.orders.aggregate( [

   // Stage 1: Filter pizza order documents by pizza size
   {
      $match: { size: "medium" }
   },

   // Stage 2: Group remaining documents by pizza name and calculate total quantity
   {
      $group: { _id: "$name", totalQuantity: { $sum: "$quantity" } }
   }
] )
```


#### Opérateur : 

 #### Liste d'opérateur possible 
 
**$ne** : different de ...
**$gt** : superieur a ... 
**$gte** : superieur ou egal ...
**$in et $nin** : absence ou presence

**$expr** permet d'utiliser des expressions dans nos requêtes .  
Ces expressions pourront contenir des operateurs, des objets ou encore des chemins d'objet pointant vers des champs

```Javascript 

db.personnes.find({
	"nom": {$exists: 1},
	"age": {$exists: 1},
	$expr: {$gt: [{$multiply: [{$strLenCP: "$nom"}, 12] }, "$age"]}
},{
 "nom": 1,
 "_id": 0
})

//le $ entre " " avec le nom d'un champs est un chemin vers ce dernier 
//Comme un this.nomDuChamps
```


```Javascript
db.banque.find(
 {  "credit": {$exists: 1}, 
	"debit": {$exists: 1},
	$expr: {$gt: [{$sum: "$debit"}, "$credit"]}
},{
 "credit": 1,
 "_id": 0,
 "debit": 1
})

```

L'operateur **$type**

```Javascript 
{champ: {$type: < type BSON >}}
{champ: {$type: < type BSON >}}

db.personnes.insertOne({
	"nom": "Zidane", "prenom": "Z", "age": numberInt(50)
})
```

L'operateur **$mod**

```Javascript

{champ: {$mod: [diviseur, reste]}}
```

L'operateur **$where** permet l'exécution direct du javascript sur la totalité des documents (Eviter l'utilisation)

```javascript 

db.personnes.find({$where: "this.nom.length > 6"})
db.personnes.find({$where: function(){
return obj.nom.length > 6
}})
//obj = this

```


Les operateur de **tableau** : 

```Javascript 

$field{$push: {champ: valeur, ...}} //ajoute des valeur dans notre tableau
db.hobbies.updateOne({"_id": 1},{$push: {"passions": "Street figth"}})
//Pour un update 1 document précis cibler sont id 
db.hobbies.updateOne({"_id": 3},{$pull: {"passions": "parachute"}})
```

L'operateur **$addToSet**  permet d'éviter l'ajout de doublon 
On peut le remplacer l'opérateur **$push** 

```Javascript 
db.personnes.find({"interets": "jardinage"}, {"_id": 1 , "nom": 1, "interets": 1 })

//retourne tout les documents comportant une liste interets avec comme valeur 
//bridge et jardinage 'l'ordre importe pas 
db.personnes.find({"interets": {$all: ["bridge", "jardinage"]}})

//Cible tout les document ayant une liste interets avec "jardinage" position 2
db.personnes.find({"interets.1": "jardinage"})

//retourne tout les document possédant le tableau intérant de taille 2
db.personnes.find({"interets": {$size: 2}})

```


L'opérateur **$elemMatch**

```Javascript 
//retourne tout ceux  ayant une  note compris en 0 et 10 
db.eleves.find({"notes": {$elemMatch: {$gt: 0 , $lt: 10}}})


//retourne tout ceux qui ont eu 5 et 7,50 ($all s'effectue comme un 'et')
db.eleves.find({"notes": {$all: [5, 7.50]}})


```

**Les tableaux  de documents** : 

```Javascript 
  db.eleves.find({"notes.note": 10})
// renvoie l'éléves ayant eu un note égal à 10 

db.eleves.find(
{"notes": 
 {$elemMatch: 
   { "note": 
     {$gt: 10, $lte: 15}
    }
  }
})
// ne prend pas les deux condition à la fois 
db.eleves.find({"notes.note": {$gt: 10, $lte: 15}})


```



**Les Index :** 

A chaque fois que on doit faire une recherche, on doit tout feuilleter pour trouver la donnée ou réponse voulu. 
L'index est comme un fichier ou un page un sommaire regroupant les informations principaux , essentiels 
Ce dernier permet de facilité la récupération de donnée. 
Indexer tout nous fait retomber à la case départ ne facilitant plus la recherche 

L'indexation a un impact négatif sur tout ce qui est écriture. 

Pour savoir quelle champs indexer, faut connaitre le métier, ou ceux qu'il vont l'employer et qui vont envoyé certaine requête. 

Algolia  || elastic search ELK  sont des outils permettant l'indexation . 

La nature de votre application devra impacter votre logique d'indexation : 
est-elle orientée écriture (write-heavy) ? ou lecture (read-heavy) ? 

Voir l'utilisation de l'indexation et essayer de la mettre en place. 


Exemple de syntaxe de création d'index : 

```javascript 
db.collection.createIndex(<champ + type> , <options?>)

db.personnes.createIndex({"age": -1})
{
	"createCollectionAutomatically": false, 
}
db.personnes.getIndexes()

db.personnes.dropIndex("nom_2")

db.personnes.createIndex({"age": -1}, {"name": "myIndex"})
db.personnes.createIndex({"age": -1}, {"name": "myIndex", "background": true, "collation": {"locale": "fr"}})

//Index Composé : 

db.personnes.createIndex({"age" : -1, "nom": 1})
```
Un index peut porter sur plus d'un champs = Index composé 
L'ordre dans lequel  les champs sont énumérer est très important

La suppression et la création d'index peut avoir un impact très négatif sur notre base de donnée .
Les procédure de gestion d'index faut mettre en place des maintenance ; 
On peut lui ajouté diverse option telle que background, collation 

Une collation est une information permettant de catégoriser une donnée  en fonction de la locale. 
Ensemble de règles permettant la gestion d'une langue.


MongoDB permet l'utilisation de deux types d'index qui permettent de gérer les requetes géospatiales : 
- Les index de type `2dsphere` sont utilises par des requêtes géospatiales intervenant sur un surface sphérique 
- les index  `2d` concernent des requêtes intervenant sur un  plan Euclidien. 

Pour un champ comme `donneeSpatiales` d'une collection `cartographie` vous pouvez par exemple créer un index de type `2d` avec la commande : 
```Javascript 
db.cartographie.createIndex({"donneesSpatiales": "2d" })
```

Pour la création d'un index `2dsphere` on utilisera plutôt : 
```Javascript 
db.cartographie.createIndex({"donneesSpatiales": "2dsphere"})
```


Les index `2d` font intervenir des coordonnes de type `legacy` 

GeoJSON permet d'écrire des données géographique en format JSON 
(Deviendra plus tard une normes. )

Permettent de travailler avec des abscices et ordonnée et latitude et longitude . 

```Javascript 
db.plan.insertOne({"nom": "firstPoint", "geodata": [1,1]})

db.plan.insertOne({"nom": "firstPoint_bis", "geodata": [4.7, 44.5]})

db.plan.insertOne({"nom": "firstPoint_bis", "geodata": ["lon": 4.7, "lat":  44.5]})
```


 **Objet GEOJSON :** 

```Shell
{type: <type dobjet>, coordinates: <coordonnees>}
```

- Le type Point : 

```Javascript 

{
	"type": "Point", 
	"coordinates": [14.0, 1.0]
}


```

- Le type MultiPoint : 

```Javascript 
{
	"type": "MultiPoint", 
	"coordinates": [
	[14.0, 1.0],  [14.0, 5.0]
	]
}

```

- Le type LineString :

```Javascript 
{
	"type": "LineString", 
	"coordinates": [
	[14.0, 1.0], [14.0, 5.0]
	]
}
```

- Le type Polygon : 
```Javascript 

{
	"type": "Polygon", 
	"coordinates": [
	[
	[14.0, 1.0], [14.0, 5.0]
	],
	[
	[14.0, 1.0], [14.0, 5.0]
	],
	]
}

{
    "type": "Polygon",
        "coordinates": [
            [
                [-35.15625, 35.460669951495305], [18.28125, -44.087585028245165],
                [59.765625, 13.239945499286312], [30.585937499999996, 33.7243396617476],
                [-35.15625, 35.460669951495305]
            ]
        ]
}
```

- Le type MultiPolygon : 

```Javascript 
{ "type": "MultiPolygon",
 "coordinates": [
    [
        [
            [-4.921875, 64.16810689799152], [-35.15625, 41.50857729743935],
            [7.3828125, 19.973348786110602], [37.6171875, 41.77131167976407],
            [44.6484375, 71.41317683396566], [16.5234375, 71.74643171904148],
            [-4.921875, 64.16810689799152]], [[-1.0546875, 53.330872983017066],
            [-9.140625, 48.69096039092549],[0.3515625, 40.713955826286046],
            [8.7890625, 41.244772343082076],[10.546875, 49.83798245308484],
            [-1.0546875, 53.330872983017066]
            ]
        ],
         [
            [
                [-22.67578125, 68.07330474079025],
                [-29.355468750000004, 64.16810689799152],
                [-18.45703125, 60.930432202923335],
                [-10.72265625, 65.36683689226321],
                [-22.67578125, 68.07330474079025]
               ]
           ]
      ]}
```


- L'operateur $nearSphere: 

```Javascript
{
	$nearSphere : {
		$geometry: {
			type: "Point",
			coordinates: [<longitude>, <latitude>]
		},
		$minDistance: <distance en metres>, // optionnel
		$maxDistance: <distance en metres>// optionnel
	}
}

{
	$nearSphere : {
	$minDistance: <distance en radians>,
	$maxDistance: <distance en radians>
	}
}
```
Nativement $nearSphere effectue un trie. 
$GeoNear existe mais il ne fait pas de trie

Test sur la collection Avignon : 

```Javascript 
//Création de notre Point operaAvignon
var operaAvignon = {type: "Point", coordinates : [4.80,43.94]}

//Trie des localisation par rapport à leur proximité de l'opéra 
db.avignon.find({"localisation": {$nearSphere: {$geometry: operaAvignon}}}, {"_id": 0 , "nom": 1})


```

- L'opérateur $geoWithin : 

Cet operateur n'effectue aucun tri et ne nécessite pas la création d'un index géospatiale, on l'utilise de la manière suivante : 
Ca permet de capture les documents dont les données géospatiales sont  al 

```Javascript 

{
	<champ des documents contenant les coordonnees> : {
		$geoWithin : {
			<operateur de forme>: <coordonnees>
		}
	}

}
```

```Javascript 
//Creation d'un polygone pour notre exemple 
var polygone = [
	[4.80,43.94],
	[4.80,43.94],
	[4.80,43.94],
	[4.80,43.94]
]
```

```javascript 

db.avignon2d.find({
	"localisation": {
		$geoWithin: {
			$polygon: polygone
		}
	}
	{"id": 0, "nom": 1 }
})
```



 #### Framework d'agrégation : 

C'est un outil d'analyse de traitement d'information 
Mise à disposition par MongoDB. 
Le pipeline d'agrégation (ou framework) 

Tapis roulant dans une usine. 

Comparable a un long tuyaux ou durant laquelle on fait subir plusieurs  étapes via des opérateur d'agrégation. 

Méthode utilisée : 

```javascript 
db.personnes.aggregate()
```

Parmi les options, nous retiendrons : 

 - Collation, permet d'affecter une collation a l'operation d'aggregation 
 - bypassDocumentValidation : fonctionne avec un operateur appele $out et permet de passer au travers de  la validation de documents
 - allowDiskUse: donne la possibilité de faire deborder les operations d'écriture sur le disque. 


```Javascript 
$match // est un filtrage permettant d'ameliorer les performance de notre requete
//Il faut penser à le definir le plus en Amont possible dans notre requete d'agregation 

//Syntaxe : 
{$match : {<requete>} }

//Exemple : 
var pipeline = [{
	$match : {
		"interets": "jardinage"
	}
}]
db.personnes.aggregate(pipeline)

var pipeline = [{
	$match : {
		"interets": "jardinage"
	},
	$match : {
		"nom": /^L/,
		"age": {$gt: 70}
	}
}]

-------------------------------------------------
//Selection/Modification de champs : $project
{ $project: { <spec> }}


//Exemple : 

var pipeline = [{
	$match : {
		"interets": "jardinage"
	},
	$project: {
		"_id": 0,
		"nom": 1,
		"prenom": 1,
		"super_senior": {$gte: ["$age",70]}
	}
},
{
	$match: {
		"super_senior": true
	}
}]

db.personnes.aggregate(pipeline)

```

 #### Opérateur : 

- $addFields

```Javascript 

db.personnes.aggregate([
{
	$addFields: {
		"numero_secu_s": ""
	}
	}
])
```

- L'operateur $group

```Javascript 
{
	$group: {
			"_id": <expression>,
			<champ>: { <operateur d'acumulation>}
	}
}

//Operateur d'accumulation: 

$push: 
$sum:
$avg:
$min:
$max:
$count:
```
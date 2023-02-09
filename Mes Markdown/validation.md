## Exercice 1 

*Modifiez la collection salle afin que soient dorénavant validés les documents destinés à y être insérés ; cette validation aura lieu en mode « strict » et portera sur les champs suivants :*
*nom sera obligatoire et devra être de type chaîne de caractères.
capacite sera obligatoire et devra être de type entier (int).*
*Dans le champ adresse, les champs codePostal et ville, tous deux de type chaîne de caractères, seront obligatoires.*

 ### Réponse : 

```Javascript 
db.runCommand(
{
	"collMod": "salles",
	"validator": {
		$jsonSchema: {
			"bsonType": "object",
			"required": ["nom", "capacite", "adresse.codePostal", "adresse.ville"],
			"properties": {"nom": {
                    "bsonType": "string",
                    "pattern": "^[A-Z].*",
                    "description": "Chaine de caractere + RegEXP - obligatoire"},
                    "capacite":{
                    "bsonType": "int",
                    "description": "Entier  - obligatoire"},
                    "adresse.codePostal": {
                    "bsonType": "string",
                    "description": "Chaine de caractere - obligatoire"},
                    "adresse.ville":{
                    "bsonType": "string",
                    "description": "Chaine de caractere - obligatoire"},
}}
}
})
```

### Que constatez-vous lors de la tentative d’insertion suivante, et quelle en est la cause ?

```
db.salles.insertOne( 
{"nom": "Super salle", "capacite": 1500, "adresse": {"ville": "Musiqueville"}} 
) 
```
![[Pasted image 20230207202556.png]]
La cause proviens du fait que on essaie d'ajouté un documents sans respecter les règles que on à établis. Il manque la valeur codePostal qui est obligatoire lors de la création de documents dans la collection.

#### Que proposez-vous pour régulariser la situation ?

Pour régularisez la situation, il faudrait simplement rajouté le codePostal car ce dernier est obligatoire.
Cette requête ci-dessous permet la création du documents demandé . 

```Javascript 
db.salles.insertOne( 
{"nom": "Super salle", "capacite": 1500, "adresse": {"ville": "Musiqueville", "codePostal": '38110'}} 
) 
```

## Exercice 2

Rajoutez à vos critères de validation existants un critère supplémentaire : le champ _id devra dorénavant être de type entier (int) ou ObjectId.

```Javascript
db.runCommand(
{
	"collMod": "salles",
	"validator": {
		$jsonSchema: {
			"bsonType": "object",
			"required": ["nom", "capacite", "adresse.codePostal", "adresse.ville"],
			"properties": {"nom": {
                    "bsonType": "string",
                    "pattern": "^[A-Z].*",
                    "description": "Chaine de caractere + RegEXP - obligatoire"},
                    "_id":{
                    "bsonType": ["int", "objectId"],
                    "description": "Int + ObjectId"},
                    "capacite":{
                    "bsonType": "int",
                    "description": "Entier  - obligatoire"},
                    "adresse.codePostal": {
                    "bsonType": "string",
                    "description": "Chaine de caractere - obligatoire"},
                    "adresse.ville":{
                    "bsonType": "string",
                    "description": "Chaine de caractere - obligatoire"},
}}
	}
})
```

#### Que se passe-t-il si vous tentez de mettre à jour l’ensemble des documents existants dans la collection à l’aide de la requête suivante :

```
db.salles.updateMany({}, {$set: {"verifie": true}}) 
```

Cela viens ajouter un champ "verifie" à true- à l'intérieur de tout mes documents présent dans ma collection. 

Supprimez les critères rajoutés à l’aide de la méthode delete en JavaScript

```Javascript 
db.salles.updateMany({
  "_id": {$exists: 1}
}, {
  $unset: {
    "verifie": ""
  }
});
```

## Exercice 3

Rajoutez aux critères de validation existants le critère suivant :

Le champ smac doit être présent OU les styles musicaux doivent figurer parmi les suivants : "jazz", "soul", "funk" et "blues".

```Javascript 

db.runCommand(
{
	"collMod": "salles",
	"validator": {
		$jsonSchema: {
			"bsonType": "object",
			"required": ["nom", "capacite", "adresse.codePostal", "adresse.ville"],
			"properties": {
				"nom": {
					"bsonType": "string",
					"pattern": "^[A-Z].*",
					"description": "Chaine de caractere + RegEXP - obligatoire"
				},
				"_id": {
					"bsonType": ["int", "objectId"],
					"description": "Int + ObjectId"
				},
				"capacite": {
					"bsonType": "int",
					"description": "Entier  - obligatoire"
				},
				"adresse.codePostal": {
					"bsonType": "string",
					"description": "Chaine de caractere - obligatoire"
				},
				"adresse.ville": {
					"bsonType": "string",
					"description": "Chaine de caractere - obligatoire"
				},
				"$or": [
					{ "smac": { "bsonType": "bool" } },
					{ "styles": { 
						"bsonType": "array",
						"items": {
							"enum": ["jazz", "soul", "funk", "blues"] 
						}
					}}
				]
			}
		}
	}
})```

Que se passe-t-il lorsque nous exécutons la mise à jour suivante ?


db.salles.update({"_id": 3}, {$set: {"verifie": false}}) 

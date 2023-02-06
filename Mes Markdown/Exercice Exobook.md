Créez une base de données sample nommée "sample_db" et une collection appelée "employees".
Insérez les documents suivants dans la collection "employees":

{
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
}

{
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
}

{
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
}

### Réponse : 
Pour ce faire j'ai tout d'abord exécuter la commande :
```Javascript 
use sample_db
```
Qui ma permis de me mettre sur ma DB sample_db me la créant par la même occasion.
Ensuite j'ai simplement effectuer : 
```javascript
db.employees.insertMany([{
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
},

{
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
},

{
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
},])

```

Grâce à cette dernière, j'ai pus créé ma Collections "employees" qui est composé de trois Documents.


## Écrivez une requête MongoDB pour trouver tous les documents dans la collection "employees".

### Réponse : 
 
 Pour obtenir la totalité des document d'une collections.
 Il suffit décrire une requête **find(query, projection)**
 Cette dernières permet d'obtenir et d'afficher les documents d'une collections.
 ```Javascript 
  db.employees.find()
// Cette requetes nous affiche la totalité de nos document de la collection employees 

//Résultat de la requêtes : 
{
   _id: ObjectId("63e1516220a28d3778a37409"),
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
},

{
   _id: ObjectId("63e1516220a28d3778a3740a"),
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
},

{
   _id: ObjectId("63e1516220a28d3778a3740b"),
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
}

```


## Écrivez une requête pour trouver tous les documents où l'âge est supérieur à 33.

### Réponse : 
Pour répondre à la demande on reprend la fonction **find(query, projection)**.
 Sauf que on va ajouté une **query** qui nous permettra d'obtenir que les documents ayant un champ "age" > 33 :
 ```javascript
db.employees.find({"age": {$gt: 33}})
//Cette requête que les documents de la collection employees qui possèdent un champs "age" supérieur à 33 
//le $gt est notre opérateur signifiant supérieur.

//Résultat de la requête : 
{
   _id: ObjectId("63e1516220a28d3778a37409"),
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
},
{
   _id: ObjectId("63e1516220a28d3778a3740b"),
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
}


```

## Écrivez une requête pour trier les documents dans la collection "employees" par salaire décroissant.

### Réponse : 
 La requête est toujours composé de  **"find()"** mais pour le coup sans compléter les paramètre attendu. 
 On va utiliser **find().sort()** nous permettant alors de trier tout nos document.
 ```Javascript 
db.employees.find().sort("salary")
//Cette nous retourne tout les documents mais les classes du plus petit salaire au plus grand. 

//Résultat de la requête : 
{
   _id: ObjectId("63e1516220a28d3778a3740a"),
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
},
{
   _id: ObjectId("63e1516220a28d3778a37409"),
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
},
{
   _id: ObjectId("63e1516220a28d3778a3740b"),
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
}

```

## Écrivez une requête pour sélectionner uniquement le nom et le job de chaque document.

### Réponse : 
 Pour obtenir uniquement le nom et le job via une requêtes . 
 On va  devoir employé **find()**. On va devoir spécifier tout les paramètre de notre fonction **find(query, projection)** afin d'obtenir le résultat attendu.
 ```Javascript 
//Donc afin de répondre à la demande la requête est : 
db.employees.find({"_id": {$exists: true}},{"_id": 0, "name": 1, "job": 1})
//Pour spécifier une projection le paramètre query se retrouve obligatoire.
//je vérifie simplement si le champs _id existe. 
//Ensuite je viens spécifier ce qui doit être afficher sachant que on ne veut pas que l'id soit affiché on est obligé de le spécifier à 0 car il s'affiche automatiquement dans le cas contraire. 


//Résultat de notre requête : 
{
  name: "John Doe",
  job: "Manager",
},
{
  name: "Jane Doe",
  job: "Developer",
},
{
  name: 'Jim Smith',
  job: "Manager",
},

```

## Écrivez une requête pour compter le nombre d'employés par poste.

### Réponse : 

Pour ce faire, j'ai employer  un **aggregate()** permettant : 
- de regroupez les valeurs de plusieurs documents.
- Effectuez  des opérations sur les données groupées pour obtenir un résultat unique 
- Analysez l'évolution des données  dans le temps 
```Javascript
db.employees.aggregate( [
{ $group: {_id: "$job", NombreEmployerParPoste: { $count: {}}}}] )
//Cette requêtes nous permet de regrouper par la clé "job" et viens ensuite compter le nombre de document possédant la clé en question.

//Résultat de la requête : 

{
 _id: 'Developer'
 NombreEmployerParPoste: 1
},
{
 _id: 'Manager'
 NombreEmployerParPoste: 2
},

```

## Écrivez une requête pour mettre à jour le salaire de tous les développeurs à 80000.

### Réponse : 
Pour ce faire, on va employé une requêtes avec la fonction updateOne permettant de modifier le salaires de tout les développeurs.

```Javascript 
db.employees.updateOne({"job": "Developer"},{
$set: {"salary": 80000}
},)
//Cette requêtes nous permet alors de modifier le salaires des développeurs pour qu'ils soient tous à 80000 de salaire
//le $set est l'opérateur permettant le changement 

//Avant la requêtes demandé

db.employees.find({"_id": {$exists: true}},{"_id": 0, "name": 1, "job": 1, "salary": 1})
//Affiche le nom, le job et le salaire de tout mes documents présent dans ma collection
{
  name: "John Doe",
  job: "Manager",
  salary: 80000,
},
{
  name: "Jane Doe",
  job: "Developer",
  salary: 75000,
},
{
  name: 'Jim Smith',
  job: "Manager",
  salary: 85000,
},

//Après ma requête demandé :

{
  name: "John Doe",
  job: "Manager",
  salary: 80000,
},
{
  name: "Jane Doe",
  job: "Developer",
  salary: 80000,
},
{
  name: 'Jim Smith',
  job: "Manager",
  salary: 85000,
},




```






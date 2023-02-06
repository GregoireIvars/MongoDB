
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






















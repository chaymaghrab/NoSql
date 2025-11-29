# Introduction au NoSQL

NoSQL (Not Only SQL) désigne une famille de bases de données non relationnelles, conçues pour gérer de grands volumes de données, non structurées ou en évolution rapide, avec haute performance et scalabilité horizontale (ajout de serveurs).
Elles offrent un schéma flexible et sont adaptées aux applications : web, mobile, temps réel, big data


## SQL vs NoSQL 


| SQL  | NoSQL  |
| :--- |:-------| 
| Données structurées (tables)  |   Données flexibles (JSON, clé-valeur, graphes…)  |  
| Schéma strict  | Schéma souple  |   
| Scalabilité verticale  | 	Scalabilité horizontale  | 
|  Très bon pour les jointures & transactions|	Très bon pour la performance & le volume |
|MySQL, PostgreSQL | Redis, MongoDB, Cassandra |


## Les quatres familles de NoSQL

```Key–Value(Clés valeurs)```

* Le modèle le plus simple
* On stocke une clé et une valeur
* Ultra rapide en lecture/écritur
Exemples : Redis, azure Cosmos DB

```Document(Documents)```

* Stocke les données sous forme de documents JSON.
* Flexible, idéal pour des données hétérogènes et des API.
* Exemples : MongoDB, CouchDB, Cassandra

```Column-Family(Les colonnes)```

* Stockage organisé en colonnes plutôt qu’en lignes.
* Optimisé pour les très grands volumes distribués.
* Exemples : Spark, HBase

```Graph Databases(Graphes)```

* Modèle basé sur des nœuds et relations.
* Parfait pour les réseaux sociaux, recommandations, navigation de graphes.
* Exemples : Neo4j, FlockDB


## Redis : c’est quoi ?

Redis = REmote DIctionary Server
* Base de données NoSQL Key-Value
* Fonctionne en RAM, ultra rapide
* Utilise des structures de données avancées :

  1.Strings
  
  2.Lists
  
  3.Sets
  
  4.Sorted Sets
  
  5.Hashes
  
  6.Pub/Sub
  
  7.Keys API
  
  8.Multiple databases

## installations de Redis avec Docker 

Téléchargez l'image de Redis depuis docker:

```bash
docker pull redis
```

Lancez le conteneur créé:

```bash 
docker run --name redis-container -d -p 6379:6379 redis
```

connexion à la CLI Redis :

```bash
docker exec -it redis-container redis-cli
```

# Commandes Redis — Gestion des clés 

## Créer / Écrire
```bash
SET clé valeur
```

## Lire une valeur
```bash
GET clé
```

## Supprimer une clé
```bash
DEL clé
```

## Lister toutes les clés
```bash
KEYS *
```

##  Définir une expiration
```bash
EXPIRE clé secondes
```

## Temps restant avant expiration
```bash
TTL clé
```




# Commandes Redis — Manipulation avancée

---

## Manipulation de nombres (Compteurs)

### ➤ Initialiser un compteur
```bash
SET visites 0
```

### ➤ Incrémenter
```bash
INCR visites
```

### ➤ Décrémenter
```bash
DECR visites
```

##  Listes (Lists)

### ➤ Ajouter dans la liste

#### Ajouter à gauche :
```bash
LPUSH maListe valeur
```

#### Ajouter à droite :
```bash
RPUSH maListe valeur
```

### ➤ Lire une liste
```bash
LRANGE maListe 0 -1
```

### ➤ Pop (retirer et retourner)

#### À gauche :
```bash
LPOP maListe
```

#### À droite :
```bash
RPOP maListe
```

## Sets (Ensembles)

➡️ Un set contient des valeurs **uniques**, dans un **ordre non garanti**.

### ➤ Ajouter
```bash
SADD monSet valeur
```

### ➤ Liste des éléments
```bash
SMEMBERS monSet
```

### ➤ Supprimer un élément
```bash
SREM monSet valeur
```

### ➤ Union entre deux sets
```bash
SUNION set1 set2
```

## Sorted Sets (ZSET) — Ensembles ordonnés

➡️ Chaque élément possède un **score**  
➡️ Les éléments sont **triés automatiquement**

### ➤ Ajouter
```bash
ZADD scores 19 "Augustin"
```

```bash
ZADD scores 18 "Inès"
```

### ➤ Lire dans l’ordre croissant
```bash
ZRANGE scores 0 -1
```

### ➤ Lire dans l’ordre décroissant
```bash
ZREVRANGE scores 0 -1
```

### ➤ Obtenir le rang (position)
```bash
ZRANK scores "Augustin"
```

## Hashes (objet JSON)

➡️ Représente un objet avec plusieurs champs internes.

### ➤ Ajouter champ par champ
```bash
HSET user:11 username "Youssef"
```

```bash
HSET user:11 age 31
```

```bash
HSET user:11 email "youssef@site.com"
```

### ➤ Ajouter plusieurs champs en une fois
```bash
HMSET user:4 username "Augustin" age 25 email "augustin@gmail.com"
```

### ➤ Lire tout l'objet
```bash
HGETALL user:4
```

### ➤ Incrémenter un champ numérique
```bash
HINCRBY user:4 age 4
```

## 📡 Pub/Sub (Temps réel)

➡️ Échange de messages entre clients en temps réel.

### ➤ S’abonner à un canal
```bash
SUBSCRIBE mesCours
```

### ➤ Publier un message
```bash
PUBLISH mesCours "Un nouveau cours MongoDB est disponible"
```

### ➤ Pattern (s’abonner à plusieurs canaux)
```bash
PSUBSCRIBE mes*
```

## Bases de données internes Redis

➡️ Redis contient **16 bases** (indexées de **0 à 15**)

### ➤ Changer de base
changer la base de 0(par defaut) à 1
```bash
SELECT 1
```


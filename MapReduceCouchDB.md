# Récap

## CouchDB
CouchDB est une base de données NoSQL orientée documents. Cela veut dire qu’au lieu de stocker les données dans des tables comme dans une base de données classique, CouchDB stocke les informations sous forme de documents JSON. Chaque document peut avoir une structure différente, ce qui rend la base très flexible.

On utilise CouchDB parce qu’il est facile à installer, simple à utiliser et bien adapté aux applications distribuées. Il permet de gérer des données sans schéma fixe et de travailler facilement avec le web, car il fonctionne directement avec le protocole HTTP. Cela le rend pratique pour les applications web et les services qui échangent des données via des API.

## Fonctionnement
Le fonctionnement de CouchDB repose sur des requêtes HTTP simples comme GET, PUT, POST et DELETE. Chaque base de données, chaque document et chaque opération sont accessibles via une adresse web. CouchDB propose aussi une interface graphique qui permet de gérer les bases et de visualiser les documents sans écrire de code. Pour effectuer des calculs ou des recherches sur les données, CouchDB utilise le principe de MapReduce.

## Utilité

L’utilité principale de CouchDB est de stocker et de manipuler facilement de grandes quantités de données non structurées. Il est surtout utile dans les applications où les données évoluent souvent, où la structure n’est pas toujours la même et où la disponibilité et la simplicité sont importantes. CouchDB permet aussi de travailler dans des environnements distribués et de gérer les pannes sans perdre les données.

# Partie 1. 

L'objectif de cette première partie est de vous initier au concept de MapeReduce, en mode centralisé, avec CouchDB. Pour cela, vous devrez visionner les deux vidéos ci-dessous et rédiger un rapport de quelques pages qui explique tous les concepts abordés,, y compris la présentation de CouchDB (première vidéo).  Votre rapport doit être accessible même pour une personne n'ayant jamais étudié le concept de MapeReduce (vous pouvez vous inspirer des exemples prséentés dans le CM).  

Aux origines du MapReduce  : soit une matrice 𝑀
 de dimension 𝑁×𝑁
 représentant des liens d'un très grand nombre de pages web (soit 𝑁
).  Chaque lien est étiqueté par un poids (son importance).  

Proposer un modèle, sous forme de ducuments structurés,  pour représenter une telle matrice (s'inspirer du cas  Page Rank du moteur de recherche Google, vu en cour). Soit 𝐶
 la collection ainsi obtenue.  
La ligne 𝑖
 peut être vue comme un vecteur à 𝑁
 dimensions décrivant la page 𝑃𝑖
.  Spécifiez le traitement MapReduce qui calcule la norme de ces vecteurs à partir des documents de la collection  𝐶
.  La norme d'un vecteur 𝑉(𝑣1,𝑣2,...,𝑣𝑁)
 est le scalaire ||𝑉||=𝑣21+𝑣22+...+𝑣2𝑁‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾√
Nous voulons calculer le produit de la matrice  𝑀
 avec un vecteur de dimension 𝑁
, 𝑊(𝑤1,𝑤2,...,𝑤𝑁)
. Le résultat est un vecteur 𝜙
 =   ∑𝑁𝑗=1𝑀𝑖𝑗𝑤𝑗
,  On suppose  que  le vecteur  𝑊
 tient en mémoire RAM et est accessible comme variable statique par toutes les fonctions de Map ou de Reduce. Spécifiez le traitement MapReduce qui implante ce calcul.

# Réponse : 
## 1 
Pour représenter la matrice M, on peut utiliser une collection de documents dans CouchDB. Chaque document représente une page web et correspond à une ligne de la matrice. Dans chaque document, on stocke la page elle-même et la liste des liens qu’elle possède vers d’autres pages, avec pour chaque lien un poids qui représente son importance. Ainsi, la collection C est l’ensemble de ces documents, et chaque document contient toutes les informations nécessaires pour décrire une ligne de la matrice.

## 2
La ligne i de la matrice peut être vue comme un vecteur qui décrit la page Pi. Pour calculer la norme de ce vecteur avec MapReduce, la phase Map parcourt chaque document et extrait les poids des liens. Pour chaque poids, on calcule son carré et on l’associe à la page correspondante. Ensuite, la phase Reduce regroupe toutes les valeurs associées à une même page, les additionne et calcule la racine carrée de la somme. Le résultat final est la norme du vecteur représentant chaque page.

## 3
Pour calculer le produit de la matrice M avec le vecteur W, on utilise aussi MapReduce. Le vecteur W est déjà disponible en mémoire. Dans la phase Map, pour chaque document représentant une page, on parcourt les liens et on multiplie le poids de chaque lien par la valeur correspondante dans le vecteur W. On associe ces résultats à la page de départ. Dans la phase Reduce, on additionne toutes les valeurs obtenues pour chaque page. Le résultat est un nouveau vecteur qui correspond au produit de la matrice M par le vecteur W.

# Partie 2 
Prenez la collection de films utilisée lors du TP n°1. L’objectif de cet exercice est de vous
familiariser avec l’écriture de fonctions MapReduce dans MongoDB. N’oubliez pas de publier
votre travail dans votre dépôt Git.
1. Compter le nombre total de films dans la collection.
```bash
map = function () {
  emit("total", 1);
};

reduce = function (key, values) {
  return Array.sum(values);
};

db.movies.mapReduce(map, reduce, { out: "total_films" });
```

2. Compter le nombre de films par genre.
```bash
map = function () {
  if (this.genres) {
    this.genres.forEach(g => emit(g, 1));
  }
};

reduce = function (key, values) {
  return Array.sum(values);
};

db.movies.mapReduce(map, reduce, { out: "films_par_genre" });
```

3. Compter le nombre de films réalisés par chaque réalisateur.
```bash
map = function () {
  if (this.directors) {
    this.directors.forEach(d => emit(d, 1));
  }
};

reduce = function (key, values) {
  return Array.sum(values);
};

db.movies.mapReduce(map, reduce, { out: "films_par_realisateur" });
```

4. Compter le nombre d’acteurs uniques apparaissant dans tous les films.
```bash
map = function () {
  if (this.cast) {
    this.cast.forEach(a => emit(a, 1));
  }
};

reduce = function (key, values) {
  return 1;
};

db.movies.mapReduce(map, reduce, { out: "acteurs_uniques" });
```

Le nombre total est obtenu avec
```bash
db.acteurs_uniques.count()
```

5. Lister le nombre de films par année de sortie.
```bash
map = function () {
  emit(this.year, 1);
};

reduce = function (key, values) {
  return Array.sum(values);
};

db.movies.mapReduce(map, reduce, { out: "films_par_annee" });

```
6. Calculer la note moyenne par film à partir du tableau grades .
```bash
map = function () {
  if (this.grades) {
    this.grades.forEach(g => emit(this.title, g.score));
  }
};

reduce = function (key, values) {
  return Array.sum(values) / values.length;
};

db.movies.mapReduce(map, reduce, { out: "note_moyenne_par_film" });

```
7. Calculer la note moyenne par genre.
```bash
map = function () {
  if (this.genres && this.grades) {
    let avg = this.grades.reduce((s,g)=>s+g.score,0) / this.grades.length;
    this.genres.forEach(g => emit(g, avg));
  }
};

reduce = function (key, values) {
  return Array.sum(values) / values.length;
};

db.movies.mapReduce(map, reduce, { out: "note_moyenne_par_genre" });

```
8. Calculer la note moyenne par réalisateur.
```bash
map = function () {
  if (this.directors && this.grades) {
    let avg = this.grades.reduce((s,g)=>s+g.score,0) / this.grades.length;
    this.directors.forEach(d => emit(d, avg));
  }
};

reduce = function (key, values) {
  return Array.sum(values) / values.length;
};

db.movies.mapReduce(map, reduce, { out: "note_moyenne_par_realisateur" });

```
9. Trouver le film avec la note maximale la plus élevée.
```bash
map = function () {
  if (this.grades) {
    this.grades.forEach(g => emit(this.title, g.score));
  }
};

reduce = function (key, values) {
  return Math.max.apply(null, values);
};

db.movies.mapReduce(map, reduce, { out: "note_max_film" });

```
pour le trie :
```bash
db.note_max_film.find().sort({ value: -1 }).limit(1)
```

10. Compter le nombre de notes supérieures à 70 dans tous les films.
```bash
map = function () {
  if (this.grades) {
    this.grades.forEach(g => {
      if (g.score > 70) emit("sup70", 1);
    });
  }
};

reduce = function (key, values) {
  return Array.sum(values);
};

db.movies.mapReduce(map, reduce, { out: "notes_sup_70" });

```
11. Lister tous les acteurs par genre, sans doublons.
```bash
map = function () {
  if (this.genres && this.cast) {
    this.genres.forEach(g => {
      this.cast.forEach(a => emit(g, a));
    });
  }
};

reduce = function (key, values) {
  return Array.from(new Set(values));
};

db.movies.mapReduce(map, reduce, { out: "acteurs_par_genre" });

```
12. Trouver les acteurs apparaissant dans le plus grand nombre de films.
```bash
map = function () {
  if (this.cast) {
    this.cast.forEach(a => emit(a, 1));
  }
};

reduce = function (key, values) {
  return Array.sum(values);
};

db.movies.mapReduce(map, reduce, { out: "films_par_acteur" });

```
```bash
db.films_par_acteur.find().sort({ value: -1 }).limit(1)
```
13. Classer les films par lettre de grade majoritaire ( A , B , C , etc.)
```bash
map = function () {
  if (this.grades) {
    this.grades.forEach(g => emit(g.grade, this.title));
  }
};

reduce = function (key, values) {
  return values;
};

db.movies.mapReduce(map, reduce, { out: "films_par_lettre" });

```
14. Calculer la note moyenne par année de sortie des films.
```bash
map = function () {
  if (this.grades) {
    let avg = this.grades.reduce((s,g)=>s+g.score,0) / this.grades.length;
    emit(this.year, avg);
  }
};

reduce = function (key, values) {
  return Array.sum(values) / values.length;
};

db.movies.mapReduce(map, reduce, { out: "note_moyenne_par_annee" });

```
16. Identifier les réalisateurs dont la note moyenne de tous leurs films est supérieure
à 80.
```bash
map = function () {
  if (this.directors && this.grades) {
    let avg = this.grades.reduce((s,g)=>s+g.score,0) / this.grades.length;
    this.directors.forEach(d => emit(d, avg));
  }
};

reduce = function (key, values) {
  return Array.sum(values) / values.length;
};

db.movies.mapReduce(map, reduce, { out: "realisateurs_notes" });

```
```bash
db.realisateurs_notes.find({ value: { $gt: 80 } })
```

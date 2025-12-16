
#Partie 1. 

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

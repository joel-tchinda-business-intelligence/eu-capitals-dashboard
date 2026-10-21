# Réflexions TD1

## 1. Ma pondération météo vs CO₂
Personnellement je donnerais un peu plus d'importance à la météo, genre 60/40. Pour une semaine de vacances, s'il pleut tous les jours le voyage est un peu raté. Par contre je n'ignorerais pas le CO₂ c-a-d si deux villes ont à peu près la même météo, je choisirais clairement la plus proche.

## 2. Fiabilité de l'acquisition (1 appel vs 26)
chaque connexion a une petite chance d'échouer (timeout, coupure réseau), et plus on en fait, plus la probabilité qu'au moins une échoue augmente. Par exemple, si une connexion a 1 % de chance d'échouer, avec 26 connexions on arrive à environ 23 % de chance qu'au moins une plante. En plus, 26 requêtes d'affilée depuis la même IP peuvent être vues comme du spam par l'API et être ralenties ou bloquées.
Le commentaire du code confirme ça : avec une seule requête groupée, il n'y a qu'une seule connexion à réussir, et c'est aussi beaucoup plus rapide.
## 3. Provenance du facteur CO₂
Avant de publier pour de vrais utilisateurs, je vérifierais la valeur dans la dernière publication officielle des facteurs DEFRA et j'indiquerais la source et l'année directement sur la page. Je ferais aussi attention au fait qu'un seul facteur pour toutes les villes est une simplification. Donc idéalement je proposerais plusieurs facteurs selon la distance, ou même le train pour les trajets courts.

## 4. Relatif vs absolu (normalisation)
non, la vraie température de la capitale moyenne ne change pas, c'est une mesure. Par contre son score change : si on ajoute une ville encore plus froide, le minimum de la liste baisse, et du coup la capitale moyenne se retrouve plus haut sur l'échelle 0-1 sans que sa météo ait bougé. Ça montre qu'un KPI construit avec une normalisation est relatif : un score de 0,8 ne veut pas dire qu'il fait beau , ça veut dire il fait plus beau que la plupart des autres villes de cette liste. Si on change la liste, on change les scores.

## 5. Ma prédiction
Je pense que Madrid va gagner. Les villes très proches (Bruxelles,...) ont un bon score CO₂ mais une météo probablement moyenne fin octobre, et les villes très au sud ont du soleil mais sont trop loin. Avec un 50-50, une ville au milieu avec une météo correcte et une distance raisonnable devrait sortir en tête.

## 6. Ma prédiction était-elle juste ?
Oui, c'est bien Madrid qui est arrivée en tête, avec un score météo de 0,81 et un score CO₂ de 0,71. Ça confirme ce que j'avais compris de la formule 50/50 : elle favorise les villes qui sont correctes sur les deux critères, plutôt que celles qui sont excellentes sur un seul.

## 7. Monitoring
## 7. Monitoring

Le seul indice visible sur la page publique, c'est la ligne « Data last updated » tout en bas, qui affiche la date `generated_at` écrite par le script à chaque exécution. Si le workflow est désactivé par GitHub ou s'il plante à chaque fois, le fichier rankings.json n'est plus mis à jour, donc cette date reste bloquée et un visiteur attentif pourrait remarquer qu'elle date de plusieurs semaines.

Mais à mon avis ce n'est pas suffisant tout seul :
- la ligne est en petit, en gris et sous les graphiques, presque personne ne va la lire ;
- un visiteur ne sait pas que les données sont censées être mises à jour chaque lundi, donc il ne peut pas savoir qu'une date ancienne est anormale ;
- et le titre dit « next week » alors que les prévisions peuvent dater d'il y a un mois, donc la recommandation devient fausse sans que rien ne l'indique.

J'ajouterais un calcul qui compare `generated_at` à la date du jour, et qui affiche un bandeau rouge bien visible en haut de la page du type « Attention : données vieilles de plus de 8 jours, la recommandation n'est peut-être plus valable » si la dernière mise à jour date de plus d'une semaine.

## 8. Extension : changement de ville de départ

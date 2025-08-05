```markdown
# Rapport de Projet Machine Learning - Alexander Gosal
## Aperçu du Projet
Contexte de la création d'un modèle de recommandation de filtrage basé sur le contenu pour la recommandation d'articles, face aux défis de l'explosion d'informations dans l'ère numérique. Avec l'apparition d'Internet et des plateformes de nouvelles en ligne, le nombre d'articles disponibles dépasse la capacité humaine à trouver et filtrer l'information pertinente. Les utilisateurs se sentent souvent coincés dans des bulles d'information, ne voyant que des angles déjà connus, sans une exploration approfondie de sujets variés. Le modèle de recommandation basé sur le contenu apparaît pour résoudre ce problème de manière plus intelligente et personnalisée. En analysant les caractéristiques du contenu telles que le sujet, les mots-clés, la structure et le style d'écriture, le modèle peut comprendre l'essence de chaque article et le faire correspondre aux préférences de l'utilisateur. Dans ce contexte, l'objectif principal est de fournir une expérience de lecture plus variée, d'accroître la connaissance de l'utilisateur en présentant des articles variés mais toujours pertinents pour leurs intérêts, ainsi que d'aider les utilisateurs à explorer des angles de vue différents, promouvant ainsi la diversité de l'information.

En plus de cela, l'utilisation du modèle de recommandation de filtrage basé sur le contenu implique également la prise en compte de la confidentialité de l'utilisateur. Ce modèle fonctionne sur la base de l'analyse du contenu existant, sans nécessiter d'informations d'identité ou d'autres comportements d'utilisateur. Cela est important pour répondre aux préoccupations de confidentialité croissantes dans l'ère numérique actuelle. En utilisant un modèle *content-based*, les utilisateurs peuvent recevoir des recommandations basées sur ce qu'ils ont déjà consommé sans nécessiter de données personnelles ou d'historique d'interaction approfondi. La pérennité de ce modèle est également garantie car il ne dépend pas des données d'interaction de l'utilisateur en continu. Cela rend le modèle de filtrage basé sur le contenu approprié pour les plateformes récemment lancées ou pour des situations où les données de l'utilisateur sont limitées. De cette manière, le filtrage de recommandation basé sur le contenu est une solution intelligente et durable pour répondre aux défis de la recherche d'information efficace, améliorer la connaissance de l'utilisateur et respecter leurs besoins de confidentialité.

## Compréhension des Affaires
#### Déclaration de Problème
Sur la base du contexte ci-dessus, le problème que ce projet peut résoudre est de savoir comment construire un système de recommandation d'articles en utilisant la méthode de filtrage basé sur le contenu pour améliorer l'expérience de lecture de l'utilisateur.
#### Objectif
L'objectif de ce projet est d'améliorer l'expérience de l'utilisateur en présentant des recommandations de contenu plus pertinentes, personnalisées et attrayantes.

## Compréhension des Données
Deskdrop est une plateforme de communication interne développée par CI&T, axée sur les entreprises utilisant Google G Suite. Parmi d'autres fonctionnalités, cette plateforme permet aux employés des entreprises de partager des articles pertinents avec leurs collègues et de collaborer autour d'eux. [CI&T Deskdrop Dataset](https://www.kaggle.com/datasets/gspmoreira/articles-sharing-reading-from-cit-deskdrop?select=users_interactions.csv) contient des échantillons de journaux de 12 mois (mars 2016 - février 2017) de la plateforme de communication interne CI&T (DeskDrop) avec 73 000 interactions utilisateur enregistrées sur 3 000 articles publiés sur la plateforme.
#### Variables dans *shared_articles.csv* :
* timestamp : identifiant unique de temps
* eventType : 'CONTENT SHARED' (Article partagé sur la plateforme et disponible pour les utilisateurs.), 'CONTENT REMOVED' (Article a été supprimé de la plateforme et n'est plus disponible pour des recommandations ultérieures.)
* contentId : identifiant unique de contenu
* authorPersonId : identifiant unique
* authorSessionId : identifiant unique de session
* authorUserAgent : informations envoyées par le navigateur web
* authorRegion : informations sur l'origine géographique de l'auteur de l'article
* authorCountry : informations sur l'origine géographique du pays de l'auteur de l'article
* contentType : 'HTML', 'RICH', 'VIDEO'
* url : url ou lien vers l'article
* title : titre de l'article
* text : contenu de l'article
* lang : langue
#### Variables dans *users_interactions.csv* :
* timestamp : identifiant unique de temps
* eventType : 'VIEW' (L'utilisateur a ouvert l'article.), 'LIKE' (L'utilisateur a aimé l'article.), 'COMMENT CREATED' (L'utilisateur a créé un commentaire sur l'article.), 'FOLLOW' (L'utilisateur a choisi d'être informé de nouveaux commentaires sur l'article.), 'BOOKMARK' (L'utilisateur a marqué l'article pour un accès facile à l'avenir.)
* contentId : identifiant unique de contenu
* personId : identifiant unique
* sessionId : identifiant unique de session
* userAgent : informations envoyées par le navigateur web
* userRegion : informations sur l'origine géographique de l'utilisateur
* userCountry : informations sur l'origine géographique du pays de l'utilisateur

## Préparation des Données
#### Sélection des caractéristiques pour créer le système de recommandation
La sélection des caractéristiques dans la création d'un système de recommandation de filtrage basé sur le contenu est très importante pour s'assurer que les recommandations générées correspondent aux préférences et aux intérêts des utilisateurs. Ces caractéristiques sont utilisées comme représentation des items de contenu (dans ce cas, des articles) dans le jeu de données. En choisissant les caractéristiques appropriées, le système peut identifier les similitudes entre les items en fonction de leurs caractéristiques, permettant ainsi de fournir des recommandations plus pertinentes.
#### Suppression des données duplicées
La suppression des données duplicées dans le jeu de données est très importante car elle peut influencer la qualité de l'analyse, du modèle et des recommandations construites sur la base de ce jeu de données.
#### Changer eventType dans le dataset users interactions en pondération
L'ajout de pondération basé sur *eventType* des utilisateurs est une approche courante dans les systèmes de recommandation basés sur le contenu pour améliorer l'exactitude et la pertinence des recommandations. La colonne *eventType* dans ce contexte fait référence aux types d'activités ou d'interactions que les utilisateurs peuvent effectuer sur la plateforme ou l'application, généralement enregistrées dans les journaux de données ou les données d'interaction. Dans Deskdrop, les utilisateurs sont autorisés à voir des articles plusieurs fois et à interagir avec eux de différentes manières (par exemple, aimer ou commenter). Par conséquent, pour modéliser les intérêts des utilisateurs sur un article spécifique, toutes les interactions effectuées par les utilisateurs dans un item sont combinées par la somme pondérée des types d'interactions. Voici la pondération basée sur les interactions des utilisateurs :
|     eventType     | score |
|:-----------------:|-------|
| 'VIEW'            | 1.0   |
| 'LIKE'            | 2.0   |
| 'BOOKMARK'        | 3.0   |
| 'FOLLOW'          | 4.0   |
| 'COMMENT CREATED' | 5.0   |
#### Fusion des deux datasets
Les deux datasets sont ensuite fusionnés sur la colonne *contentId* pour obtenir *rating* afin que toutes les *rating* utilisateur d'un contenu donné puissent être cumulées pour l'évaluation du système de recommandation.
#### Somme totale des rating utilisateur pour un contenu donné pour l'évaluation
Le modèle de filtrage basé sur le contenu est un type de modèle de recommandation qui se fonde sur les caractéristiques ou les attributs de l'item lui-même, tels que le texte, les caractéristiques ou d'autres métadonnées. Dans le filtrage basé sur le contenu, l'objectif est de trouver des items qui ont des similitudes dans des attributs ou des caractéristiques spécifiques avec l'item que l'utilisateur aime. Cela signifie que le modèle essaie de comprendre les préférences de l'utilisateur sur la base de ce qu'ils aiment sur les items précédents, pas en comparant les comportements de l'utilisateur avec ceux d'autres utilisateurs. Par conséquent, tout le jeu de données est fusionné sur le titre et le nombre total de rating utilisateur pour ce titre pour l'évaluation.
###### Description du jeu de données
|        | title | text |   rating  |
|:------:|:-----:|:----:|:---------:|
|  count |  2173 | 2173 |    2173   |
| unique |  2173 | 2169 |    NaN    |
|  mean  |  NaN  |  NaN | 20.936954 |
|   min  |  NaN  |  NaN |     0     |
|   max  |  NaN  |  NaN |    462    |

La moyenne des rating globaux pour tous les articles est de 20.9369 ou environ 21. Par conséquent, ce rating sera utilisé comme seuil pour l'évaluation.

## Modélisation
Le modèle utilisé pour ce projet est l'utilisation de l'approche de filtrage basé sur le contenu en utilisant TfidfVectorizer. Le filtrage basé sur le contenu est une approche dans les systèmes de recommandation qui se concentre sur les caractéristiques ou le contenu des items qui seront recommandés aux utilisateurs. Dans ce cas, le modèle mesure l'identité entre les items en fonction des caractéristiques ou des attributs qu'ils contiennent. La technique utilisée dans le filtrage basé sur le contenu est *TF-IDF Vectorizer* et *Cosine Similarity*.
#### TF-IDF Vectorizer
TF-IDF est l'abréviation de Term Frequency-Inverse Document Frequency. Il s'agit d'une méthode utilisée pour mesurer l'importance d'une phrase dans un document ou un corpus. La fréquence de terme (TF) mesure à quelle fréquence une phrase spécifique apparaît dans le document. Cela donne un poids plus élevé aux phrases qui apparaissent plus souvent. L'inverse de la fréquence de document (IDF) mesure l'importance d'une phrase dans l'ensemble du corpus de documents. Les phrases qui apparaissent dans de nombreux documents obtiennent une valeur IDF plus basse. TF-IDF combine ces deux concepts pour fournir une représentation numérique des phrases dans le document.
#### Cosine Similarity
La similarité cosinus est une méthode pour mesurer l'identité entre deux vecteurs dans un espace de dimension multiple, comme dans le cas de la représentation TF-IDF des documents.
#### Résultat de recommandation
En utilisant l'article "An operating model for company-wide agile development", voici les résultats de recommandation de filtrage basé sur le contenu :
|    |                                      title                                      | rating |
|----|:-------------------------------------------------------------------------------:|--------|
| 1  | Embracing Agile                                                                 | 188.0  |
| 2  | Agile is Dead, Long Live Continuous Delivery - Gradle                           | 24.0   |
| 3  | 12 Agile principles                                                             | 1.0    |
| 4  | Scrum Community - Scrum Alliance                                                | 64.0   |
| 5  | Big IT Rising                                                                   | 50.0   |
| 6  | Organizing for digital acceleration: Making a two-speed IT operating model work | 28.0   |
| 7  | How enterprise architects can help ensure success with digital transformations  | 57.0   |
| 8  | The new tech talent you need to succeed in digital                              | 34.0   |
| 9  | Do you want Crappy Agile?                                                       | 43.0   |
| 10 | Five questions boards should ask about IT in a digital world                    | 58.0   |

## Évaluation
#### Precision@k
Precision@k est une métrique d'évaluation utilisée pour évaluer la performance d'un modèle dans les systèmes de recommandation, y compris le modèle de filtrage basé sur le contenu. Dans ce contexte, le filtrage basé sur le contenu est une approche dans les systèmes de recommandation qui analyse le contenu ou les caractéristiques des items qui seront recommandés aux utilisateurs. *Precision@k* mesure dans quelle mesure les items recommandés par le modèle sont véritablement pertinents pour les utilisateurs. Cette métrique fournit des informations sur la manière dont le modèle est précis dans l'identification des items qui sont véritablement pertinents pour les préférences ou les intérêts de l'utilisateur.
* *Precision*: *Precision* est le rapport entre le nombre d'items pertinents qui sont véritablement recommandés (*true positives*) divisé par le total du nombre d'items recommandés (*true positives* + *false positives*). *Precision* = (Nombre d'items pertinents recommandés) / (Total du nombre d'items recommandés)
* @k: La valeur "k" fait référence au nombre d'items en tête recommandés par le modèle.

L'évaluation de la précision est basée sur les rating d'un article donné. Si l'article recommandé par le système a un rating supérieur à 21, alors le contenu est considéré comme pertinent. Sinon, l'article n'est pas considéré comme pertinent.

L'évaluation de la précision@k sur le modèle :
Sur la base des résultats de recommandation d'articles avec le titre "An operating model for company-wide agile development", les articles considérés comme pertinents sont ceux qui ont un rating total de 21 ou une moyenne de rating globale. Le modèle de filtrage basé sur le contenu recommande 10 items à un utilisateur. Parmi les 10 items, 9 sont considérés comme pertinents selon les préférences de l'utilisateur.

La précision@k dans ce cas est la précision@10, et sa valeur est 9/10 = 0,9 ou 90%.

Cela signifie que, parmi les 10 items recommandés, 90% sont des items qui correspondent aux préférences de l'utilisateur. Plus la valeur de Precision@k est élevée, meilleure est la performance du modèle de filtrage basé sur le contenu dans la recommandation d'items pertinents aux utilisateurs.

## Références
* Arie Satia Dharma, Russel Buena Basadena Ayub Hutasoit, Rade Rido Pangaribuan, "Système de recommandation utilisant le filtrage item-based sur le contenu des articles de nouvelles", vol 2, No 1 (avril 2021)
* Dewa Ayu Putri Diah Pramestia et I Wayan Santiyasa, "Application de la méthode de filtrage basé sur le contenu dans le système de recommandation de jeux vidéo", Volume 1, Numéro 1, (novembre 2022) 
--------------------------------
```
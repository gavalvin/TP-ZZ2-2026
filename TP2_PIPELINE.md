# TP 2 — Préparer les données et mener l'enquête · 2 h

Vous avez un environnement opérationnel. Votre mission change : **chercher un comportement inhabituel dans les données**, puis préparer les tables qui permettront de l'examiner dans Power BI. Le réseau, le jour et la cause ne sont pas donnés.

Ouvrir `TP2_PIPELINE.ipynb`. Travailler sur le semestre complet. Tous les traitements sont en SQL ; aucun graphique n'est demandé.

## 1. Comprendre ce qu'il faut nettoyer — 25 min

Faire le bilan des valeurs manquantes, négatives, non convertibles et des doublons. Conserver une copie logique brute avant de transformer.

Le fichier contient des champs numériques lus comme texte, des dates américaines, des valeurs non renseignées, des productions **nettes** négatives et des colonnes de variantes ajustées/imputées à ne pas mélanger. Une valeur négative n'est donc pas automatiquement une erreur. Ne pas inventer des corrections.

La clé est réseau + heure UTC. L'heure locale est une **fin d'intervalle**. Le semestre traverse un changement d'heure et couvre plusieurs fuseaux : il n'existe pas un décalage UTC unique pour toute la table. Un nombre de lignes local différent de 24 n'est pas, à lui seul, un trou de données.

## 2. Construire une couche propre — 30 min

Suivre le contrat des tables dans le notebook : `typee`, `qualifiee`, `reseau_horaire`, `rejets_dates`.

Nettoyer signifie convertir les champs, nommer et sélectionner les colonnes utiles, conserver des clés fiables et distinguer les observations utilisables des observations à interpréter. Documenter chaque règle et son effet. Les mesures absentes restent absentes ; les zéros plausibles sont conservés. Les valeurs extrêmes ne sont pas supprimées par défaut.

Si un contrôle ne trouve aucun doublon ou rejet, le noter : il n'est pas nécessaire d'en fabriquer.

## 3. Ouvrir la chasse au trésor — 35 min

Rechercher des variations rapides, en tenant compte du cycle quotidien. Comparer uniquement des heures consécutives d'un même réseau. Calculer les fenêtres avant de filtrer.

Choisir un candidat et examiner les heures avant/après. Vérifier ce qui se passe dans les autres filières ainsi que dans la demande. Comparer à d'autres jours. Écrire deux hypothèses et les observations qui les soutiennent ou les fragilisent. À ce stade, une hypothèse n'est pas encore une explication démontrée.

L'enseignant peut donner des indices progressivement. Une recherche Web donnant un nom d'événement ne remplace pas les preuves dans vos données.

## 4. Préparer la sortie — 25 min

Créer `dim_temps` et `fact_production`, puis vérifier l'unicité des clés et la jointure. Le format long doit permettre à Power BI de filtrer réseau, date et filière sans refaire le nettoyage.

Exporter les trois CSV du notebook. Compter environ 2,4 millions de lignes pour la table de faits nationale à neuf filières ; ne pas afficher cette table dans le notebook. Pour un poste Power BI limité, filtrer **après l'enquête** les exports sur le réseau retenu et quelques semaines utiles, et annoncer explicitement ce périmètre.

## Pour aller plus loin — facultatif

Quatre bonus dans le notebook, avant l'export : demandes négatives, part du solaire par mois, nombre de réseaux touchés par jour par une baisse brusque du solaire, et cohérence entre production nette et somme des filières. Le bonus 3 est utile pour tester vos hypothèses : une cause unique à un seul réseau ou un phénomène étendu ? Chaque règle ou décision prise doit être documentée comme dans le reste du TP.

## 5. Sauvegarder — 5 min

```bash
git add TP2_PIPELINE.ipynb
git commit -m "TP2 : pipeline et observations"
```

**À rendre :** notebook SQL complété, règles de qualité, trois fichiers CSV et note d'enquête. Ajouter des commits intermédiaires après le nettoyage et après les premières observations.

Pour récupérer les fichiers dans Colab navigateur : onglet Fichiers → `exports` → Télécharger. Depuis VS Code, les fichiers sont distants ; si l'extension ne permet pas de les récupérer, ouvrir les notebooks dans Colab navigateur et les réexécuter. Les deux interfaces ne partagent pas nécessairement la même session.

**IA et vérification :** noter les aides utilisées. Vous devez justifier une règle de nettoyage, modifier un filtre ou une requête devant l'enseignant et expliquer pourquoi votre candidat est intéressant. L'usage d'IA ne se déduit pas d'une réponse isolée.

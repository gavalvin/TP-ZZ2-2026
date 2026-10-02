# TP 3 — Construire le dashboard d'enquête · 2 h

Vous disposez maintenant de données préparées. Construire un rapport qui permet à une autre personne de **voir l'anomalie, comparer les filières et suivre votre raisonnement**. La conclusion ne doit pas être seulement écrite dans un titre : les visuels doivent l'étayer.

Prévoir Power BI Desktop sur Windows. Les utilisateurs Mac doivent disposer d'un poste ou d'un environnement Windows pour cette séance.

## 1. Importer et vérifier — 20 min

Importer `dim_temps.csv`, `fact_production.csv` et `reseau_horaire.csv` : Obtenir des données → Texte/CSV. Choisir UTF-8 et le séparateur `;`.

Dans Power Query : clé, réseau et filière en texte ; `date_jour` en Date ; fins d'intervalle en Date/Heure ; mesures en nombre décimal. Pour les décimales avec point, utiliser les paramètres régionaux Anglais (États-Unis). Conserver les cases vides. Vérifier les types avant « Fermer et appliquer ».

Le nettoyage métier a déjà été fait en SQL. Si une erreur apparaît, corriger le pipeline puis réexporter.

## 2. Relier les tables — 15 min

Créer une relation **1 vers plusieurs**, sens unique :

`dim_temps[cle_heure]` → `fact_production[cle_heure]`.

La dimension contient un réseau et une heure par ligne. Ne pas relier seulement sur l'heure : plusieurs réseaux partagent le même horaire. Garder `reseau_horaire` comme table de contrôle indépendante dans cette première version, avec ses propres filtres. Retirer les relations automatiques ambiguës.

## 3. Créer les mesures — 15 min

Créer ces mesures séparément dans la table de faits. Les noms de table doivent correspondre à ceux de votre import.

```dax
Puissance moyenne MW =
AVERAGEX(
    VALUES(dim_temps[cle_heure]),
    CALCULATE(SUM(fact_production[puissance_mw]))
)
```

```dax
Energie MWh = SUM(fact_production[puissance_mw])
```

```dax
Mesures absentes =
COUNTROWS(fact_production) - COUNT(fact_production[puissance_mw])
```

L'énergie est ici la somme de puissances moyennes sur des intervalles d'une heure ; cette formule ne serait plus valable telle quelle avec un autre pas. Les absences ne sont pas des zéros : la somme porte seulement sur les données renseignées.

**Pour ce rapport, imposer un seul réseau sélectionné.** La mesure de puissance moyenne est prévue pour comparer ses filières dans le temps, pas pour additionner plusieurs réseaux. Les filtres de `dim_temps` ne pilotent pas automatiquement la table de contrôle indépendante.

## 4. Construire deux pages — 45 min

### Page « Repérer »

- Segments : réseau (sélection unique) et date.
- Courbe d'une filière choisie : heure locale en abscisse, puissance moyenne en ordonnée.
- Matrice ou tableau permettant de comparer plusieurs journées.
- Effectifs ou indicateur de mesures absentes.

Choisir l'affichage qui rend le candidat visible sans masquer les autres observations. Pour les courbes, sélectionner le champ Date/Heure lui-même plutôt que la hiérarchie automatique.

### Page « Comprendre »

- Courbes des filières sur le créneau retenu.
- Tableau des valeurs avant, pendant et après, calculé ou vérifié avec les requêtes SQL.
- Un visuel de comparaison avec des jours voisins.
- Trois observations chiffrées et une limite.

Utiliser des unités cohérentes. Une somme de MW sur plusieurs heures ne doit pas être présentée comme une puissance instantanée. Nommer les axes et préciser qu'il s'agit de fins d'intervalles en heure locale.

## 5. Vérifier et conclure — 25 min

Retrouver dans Power BI trois valeurs calculées en SQL, avec le même réseau et le même créneau. Vérifier qu'un changement de filtre produit le résultat attendu. Attention aux visuels issus de tables indépendantes : leur appliquer explicitement le même périmètre.

Chercher ensuite une source indépendante pour tester votre explication. Écrire un verdict court : phénomène observé, chiffres, réaction des autres filières, explication étayée et limite. Une différence avec un jour voisin ne suffit pas à prouver une cause.

**À rendre :** `.pbix`, capture des deux pages et verdict de 150–250 mots. Sauvegarder les exports séparément. Le `.pbix` peut être remis sur la plateforme de cours ; ne pas envoyer des fichiers volumineux sur Git sans consigne.

Chaque étudiant présentera un filtre en direct et expliquera une valeur. Le rapport est évalué sur la fiabilité du modèle et le raisonnement, pas sur le nombre de graphiques.

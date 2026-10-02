# TP 1 — Mettre en place son atelier data · 2 h

Aujourd'hui, vous installez votre environnement de travail et vous vous connectez à un jeu de données public. Vous savez déjà écrire du SQL : l'objectif est de faire fonctionner toute la chaîne dans VS Code.

## 1. Récupérer le projet — 20 min

Le dépôt du TP est disponible sur GitHub : <https://github.com/gavalvin/TP-ZZ2-2026>. Dans un terminal **sur votre ordinateur** :

```bash
git clone https://github.com/gavalvin/TP-ZZ2-2026.git
cd TP-ZZ2-2026
code .
```

Si `code` n'est pas reconnu, ouvrir VS Code puis Fichier → Ouvrir un dossier.

Créer sa branche : `git switch -c NOM-Prenom`.

## 2. Connecter VS Code à Colab — 25 min

Installer les extensions **Jupyter (Microsoft)** et **Colab (Google)**. Ouvrir `TP1_ENVIRONNEMENT.ipynb`, choisir le noyau en haut à droite → Colab → Auto Connect et se connecter à Google.

Le notebook est enregistré dans votre dossier Git local. Son code s'exécute sur une machine distante. Les téléchargements et la base DuckDB se trouvent sur cette machine, pas automatiquement dans le dossier local. Une session Colab peut disparaître : un notebook reproductible doit pouvoir recréer ses données.

Une cellule contient soit du texte explicatif, soit du code. `Maj + Entrée` exécute la cellule. Les variables existent après exécution, pas simplement parce que le code est écrit dans le fichier.

## 3. Faire fonctionner Python et SQL — 25 min

Exécuter les cellules fournies dans l'ordre : identifier le dossier d'exécution, installer DuckDB, activer `%%sql`, tester une requête. Ne pas ajouter d'autre extension SQL au notebook.

DuckDB est le moteur d'analyse ; Colab fournit la machine ; VS Code est l'éditeur. Les utilitaires Python sont fournis, les requêtes sont à coder en SQL.

## 4. Se connecter aux données et écrire trois requêtes — 40 min

Source : [EIA-930, premier semestre 2024](https://www.eia.gov/electricity/gridmonitor/sixMonthFiles/EIA930_BALANCE_2024_Jan_Jun.csv).

Environ **42 Mo, 266 000 lignes et 61 réseaux** dans la copie vérifiée. Garder tout le semestre et tous les réseaux. Une ligne correspond à un réseau et une heure ; plusieurs colonnes décrivent des mesures électriques. Les volumes exacts peuvent évoluer si la source est révisée.

Télécharger puis écrire les trois requêtes du notebook : inventaire, répartition par réseau, sélection de dix lignes. Ne pas créer de graphique. Ne pas charger toute la table dans pandas pour l'analyser : le traitement est effectué par DuckDB.

## Pour aller plus loin — facultatif

Si vous avez terminé avant les autres, traiter les requêtes D, E et F du notebook : réseaux qui ne déclarent pas leur demande, couverture temporelle et nombre de lignes par réseau, puis demande moyenne et pointe. Elles ne remplacent pas les trois requêtes obligatoires et ne dispensent pas du commit demandé. Compléter aussi la cellule de notes sur l'usage de l'IA.

## 5. Sauvegarder son travail — 10 min

Enregistrer le notebook puis, dans le terminal local :

```bash
git status
git add TP1_ENVIRONNEMENT.ipynb
git commit -m "TP1 : environnement et premières requêtes"
```

Avant le commit, utiliser « Clear All Outputs » si les sorties sont volumineuses. Ne pas committer les CSV ni la base. Le `.gitignore` fourni les exclut dans `data/` et `exports/`.

**À rendre :** notebook exécuté pendant la séance, réponses aux cinq phrases finales et commit. Vous devez pouvoir expliquer où s'exécute le code et modifier le filtre d'une requête.

**IA :** autorisée comme aide au dépannage si son usage est indiqué dans une cellule de notes (question posée, aide retenue, vérification effectuée). Une réponse générée doit être vérifiée. Le travail sera aussi évalué par une courte modification en direct.

# Cap. – Suivi de perte de poids

Une appli web légère pour suivre sa perte de poids : poids, pas, tour de taille, masse grasse, eau, sommeil, séances de sport.
Un seul fichier (`index.html`), aucune dépendance, aucun compte, aucun serveur.


## Musculation et thème sombre

- **Thème sombre par défaut** (Réglages → Apparence : sombre, clair ou automatique).
- **Onglet Muscu** : programme généré selon le nombre de jours (2 à 4), le niveau et le lieu (salle ou maison), séances avec saisie des séries (kg × répétitions), minuteur de repos, suggestion de progression (double progression), remplacement d'un exercice, historique et meilleures charges.
- **Bibliothèque de 23 exercices** avec illustrations schématiques (position de départ et d'arrivée), consignes, erreurs fréquentes et lien de recherche vidéo.
- Les séances sont incluses dans l'export/import JSON.

## Fonctions

- **Bilan** : poids actuel, variation depuis le départ, reste à perdre, IMC, moyenne de pas sur 7 jours, série de jours d'objectif de pas, séances de la semaine.
- **Graphiques** : courbe de poids avec moyenne sur 7 jours et ligne d'objectif, pas des 14 derniers jours, tour de taille.
- **Tendance** : vitesse de perte en kg/semaine (régression sur 4 semaines) et date estimée d'arrivée à l'objectif. Un avertissement s'affiche si la perte dépasse environ 1 % du poids par semaine.
- **Repères énergie** : métabolisme de base (Mifflin-St Jeor), dépense totale estimée, cible modérée (déficit d'environ 400 kcal, jamais sous le métabolisme de base) et repère protéines.
- **Coach** (onglet dédié) : une conversation qui guide la personne.
  - *Mode local (par défaut)* : parcours guidé (sexe, âge, taille, poids, objectif, activité) qui crée le profil et un plan de départ ; réponses sur le bilan, l'alimentation, les protéines, les pas, la musculation, les plateaux et la motivation, à partir des données saisies. Aucune donnée n'est envoyée.
  - *Mode IA (optionnel)* : l'utilisateur colle sa propre clé API Anthropic dans les réglages du coach ; les messages, avec un résumé de ses données, partent alors directement du navigateur vers l'API pour des réponses libres. Si l'appel échoue, le coach local prend le relais.
  - *Garde-fous* : refus des objectifs sous un IMC de 18,5 et des rythmes supérieurs à environ 1 % du poids par semaine ; calories jamais sous le métabolisme de base ; message d'écoute et orientation vers un professionnel si la personne évoque des comportements alimentaires à risque (ces messages sont toujours traités en local, même en mode IA) ; renvoi vers un médecin en cas de symptômes, de traitement ou de grossesse.
- **Données** : export JSON et CSV, import JSON, données de démonstration, suppression complète.
- Thème clair/sombre automatique, pensé pour le téléphone.

## Vie privée

Toutes les données restent dans le navigateur (`localStorage`). Rien n'est envoyé nulle part.
Exporte régulièrement (onglet **Données**) pour garder une copie ou changer d'appareil.

## Mode IA : sécurité et limites

- GitHub Pages n'héberge que des fichiers statiques : il n'y a **aucun serveur** pour garder une clé secrète. N'écris donc **jamais ta clé dans `index.html`** : elle serait publique.
- Dans l'appli, chaque personne utilise **sa propre clé**, stockée seulement dans son navigateur (elle n'est pas incluse dans les exports). Les appels sont facturés sur son compte.
- Pour offrir l'IA à d'autres personnes sans leur demander de clé, il faut un petit serveur intermédiaire (une fonction serverless par exemple) qui détient la clé, limite l'usage et relaie les messages. L'appel se fait dans la fonction `askAI` d'`index.html` (constantes `AI_URL`, `AI_VERSION` et `AI_MODEL` en haut de la section coach).
- L'appel direct depuis le navigateur utilise l'en-tête `anthropic-dangerous-direct-browser-access`. Si ton navigateur ou ton réseau le bloque, l'appli le signale et retombe sur le coach local.

## Publier sur GitHub Pages

### Depuis le site github.com (marche aussi sur iPhone)

1. Crée un dépôt (bouton **New**), par exemple `cap`, en public.
2. **Add file → Create new file**, nomme-le `index.html`, colle le contenu du fichier, puis **Commit changes**.
3. Fais pareil avec `README.md` (facultatif).
4. **Settings → Pages**. Sous *Build and deployment*, choisis **Deploy from a branch**, branche `main`, dossier `/ (root)`, puis **Save**.
5. Après une minute environ, l'appli est en ligne à l'adresse `https://TON-PSEUDO.github.io/cap/`.

### En ligne de commande

```bash
git init && git add index.html README.md && git commit -m "Cap, suivi de perte de poids"
git branch -M main
git remote add origin https://github.com/TON-PSEUDO/cap.git
git push -u origin main
```
Puis active Pages comme à l'étape 4.

## L'utiliser comme une appli sur iPhone

Ouvre l'adresse dans Safari, touche **Partager**, puis **Sur l'écran d'accueil**. Les données restent propres à ce navigateur : exporte avant de vider les données de Safari.

## Personnaliser

Tout est dans `index.html` : les couleurs sont les variables CSS en haut du fichier (`--brand`, `--accent`…), le calcul de la cible calorique est dans la fonction `energyPlan`.

## Avertissement

Cap n'est pas un dispositif médical. Les calculs sont des estimations. En cas de maladie, de symptômes persistants, de traitement ou de grossesse, demande l'avis d'un professionnel de santé avant de changer d'alimentation ou d'entraînement.

# 📚 Tutoriel GitHub Actions pour Débutants

Bienvenue dans ce tutoriel complet sur GitHub Actions ! Ce guide vous apprendra à automatiser vos projets étape par étape.

## 📖 Table des Matières

1. [Qu'est-ce que GitHub Actions ?](#quest-ce-que-github-actions-)
2. [Concepts de Base](#concepts-de-base)
3. [Votre Premier Workflow](#votre-premier-workflow)
4. [Exemples Pratiques](#exemples-pratiques)
5. [Ressources Utiles](#ressources-utiles)

---

## Qu'est-ce que GitHub Actions ?

**GitHub Actions** est un outil d'automatisation intégré à GitHub qui permet de :
- ✅ Exécuter des tests automatiquement
- 🚀 Déployer votre code
- 🔧 Automatiser des tâches répétitives
- 📦 Construire et publier des packages

### Pourquoi utiliser GitHub Actions ?

- **Gratuit** : 2000 minutes/mois pour les dépôts publics
- **Intégré** : Directement dans GitHub, pas besoin d'outils externes
- **Flexible** : Supporte tous les langages de programmation
- **Communautaire** : Des milliers d'actions prêtes à l'emploi

---

## Concepts de Base

### 1. Workflow (Flux de travail)
Un **workflow** est un processus automatisé que vous configurez dans votre dépôt.

### 2. Event (Événement)
Un **event** est ce qui déclenche un workflow. Exemples :
- `push` : Quand vous poussez du code
- `pull_request` : Quand une PR est créée
- `schedule` : À une heure précise (comme un cron)

### 3. Job (Tâche)
Un **job** est un ensemble d'étapes qui s'exécutent sur la même machine.

### 4. Step (Étape)
Une **step** est une action individuelle (commande ou action réutilisable).

### 5. Runner (Exécuteur)
Un **runner** est un serveur qui exécute vos workflows (Ubuntu, Windows, macOS).

### Structure d'un Workflow

```
Workflow
  └── Event (déclencheur)
       └── Job 1
            ├── Step 1
            ├── Step 2
            └── Step 3
       └── Job 2
            ├── Step 1
            └── Step 2
```

---

## Votre Premier Workflow

### Étape 1 : Créer la Structure

Dans votre dépôt GitHub, créez cette structure de dossiers :

```
votre-projet/
  └── .github/
       └── workflows/
            └── hello-world.yml
```

### Étape 2 : Créer le Fichier Workflow

Les workflows sont des fichiers **YAML** (`.yml` ou `.yaml`).

**Exemple simple** : `.github/workflows/hello-world.yml`

```yaml
# Nom du workflow (apparaît dans l'interface GitHub)
name: Mon Premier Workflow

# Déclencheur : quand déclencher ce workflow ?
on:
  push:  # Se déclenche à chaque push
    branches:
      - main  # Seulement sur la branche main

# Les tâches à exécuter
jobs:
  # Nom de la tâche
  dire-bonjour:
    # Système d'exploitation à utiliser
    runs-on: ubuntu-latest

    # Les étapes de cette tâche
    steps:
      # Étape 1 : Afficher un message
      - name: Dire bonjour
        run: echo "Bonjour le monde !"

      # Étape 2 : Afficher la date
      - name: Afficher la date
        run: date
```

### Étape 3 : Tester

1. Créez ce fichier dans votre dépôt
2. Faites un `git push`
3. Allez dans l'onglet **Actions** de votre dépôt GitHub
4. Vous verrez votre workflow s'exécuter ! 🎉

---

## Exemples Pratiques

### Exemple 1 : Tester du Code Python

```yaml
name: Tests Python

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      # Récupérer le code du dépôt
      - name: Récupérer le code
        uses: actions/checkout@v4

      # Installer Python
      - name: Installer Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      # Installer les dépendances
      - name: Installer les dépendances
        run: |
          pip install -r requirements.txt

      # Exécuter les tests
      - name: Exécuter les tests
        run: |
          pytest
```

### Exemple 2 : Déploiement Automatique

```yaml
name: Déploiement

on:
  push:
    branches: [ main ]

jobs:
  deployer:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Construire le projet
        run: npm run build

      - name: Déployer
        run: |
          echo "Déploiement en cours..."
          # Vos commandes de déploiement ici
```

### Exemple 3 : Tâches Programmées

```yaml
name: Sauvegarde Quotidienne

on:
  schedule:
    # Tous les jours à 2h du matin (UTC)
    - cron: '0 2 * * *'

jobs:
  sauvegarde:
    runs-on: ubuntu-latest

    steps:
      - name: Créer une sauvegarde
        run: |
          echo "Sauvegarde en cours..."
          # Vos commandes de sauvegarde
```

---

## Syntaxe YAML - Rappels Importants

### ⚠️ Indentation
YAML utilise l'indentation (2 espaces) pour la structure :

```yaml
jobs:          # Niveau 0
  mon-job:     # Niveau 1 (2 espaces)
    runs-on:   # Niveau 2 (4 espaces)
```

### 📝 Listes

```yaml
# Avec tirets
branches:
  - main
  - develop

# En ligne
branches: [ main, develop ]
```

### 💬 Chaînes de caractères

```yaml
# Simple
name: Mon workflow

# Multi-lignes
run: |
  echo "Ligne 1"
  echo "Ligne 2"
```

---

## Variables et Secrets

### Variables d'Environnement

```yaml
jobs:
  mon-job:
    runs-on: ubuntu-latest
    env:
      MA_VARIABLE: "valeur"

    steps:
      - name: Utiliser la variable
        run: echo $MA_VARIABLE
```

### Secrets (Données Sensibles)

1. Allez dans **Settings** > **Secrets and variables** > **Actions**
2. Créez un secret (ex: `API_KEY`)
3. Utilisez-le dans votre workflow :

```yaml
steps:
  - name: Utiliser un secret
    run: echo ${{ secrets.API_KEY }}
```

---

## Ressources Utiles

- 📘 [Documentation officielle](https://docs.github.com/actions)
- 🔍 [Marketplace GitHub Actions](https://github.com/marketplace?type=actions)
- 💡 [Exemples de workflows](https://github.com/actions/starter-workflows)
- 🎓 [GitHub Skills](https://skills.github.com/)

---

## 🎯 Exercices Pratiques

1. Créez un workflow qui s'exécute à chaque push
2. Ajoutez un job qui affiche la date et l'heure
3. Créez un workflow qui s'exécute sur plusieurs systèmes d'exploitation
4. Programmez un workflow qui s'exécute tous les lundis

---

## 📞 Besoin d'Aide ?

- Consultez l'onglet **Actions** de votre dépôt pour voir les logs
- Vérifiez l'indentation de votre fichier YAML
- Regardez les exemples dans ce dépôt

**Bon apprentissage avec GitHub Actions ! 🚀**

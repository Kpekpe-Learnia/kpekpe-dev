## Introduction

Bienvenue sur le projet **Kpekpe**.

Ce document définit les standards de développement, les conventions de code et les règles de collaboration afin de garantir un code propre, maintenable et scalable.


## 🧱 Structure du projet
L'architecture est générée via un standard Django pour garantir la séparation des préoccupations :

`kpekpe_learnia/` → Cœur du projet Django.

`config/` : Configuration globale (settings, wsgi, asgi, urls).

`users/` : Application Django dédiée à la gestion des utilisateurs.

`docs/` → Documentation technique générée via MkDocs.

`Dockerfile` / `docker-compose.yml` → Fichiers de conteneurisation pour garantir la reproductibilité.

`uv.lock` → Fichier critique. Verrouillage des versions exactes pour la reproductibilité.

`conftest.py` / `pytest.ini` → Configuration des tests automatisés.

---

## 🧑‍💻 Conventions de code

### 🔤 Naming conventions

| Type                | Convention | Exemple           |
| ------------------- | ---------- | ----------------- |
| Variables           | snake_case  | `user_name`        |
| Fonctions           | snake_case  | `get_user_data()` |
| Classes             | PascalCase | `UserService`     |
| Interfaces / Types  | PascalCase | `UserResponse`    |
| Constantes globales | UPPER_CASE | `API_URL`         |
| Fichiers            | snake_case | `user_service.py` |
| Dossiers Python (packages) | snake_case | `kpekpe_learnia/`, `users/` |
| Dossiers frontend (composants) | kebab-case | `auth-module/` |
| Dossiers config / docs | kebab-case | `docs-api/` |
---

### 📏 Règles générales et normes professionnelles

* Code en **anglais uniquement**
* Fonctions courtes et lisibles
* Pas de logique complexe inline
* Respect du principe **KISS (Keep It Simple, Stupid)**
* Respect du principe **DRY (Don't Repeat Yourself)**

Chaque ligne poussée sur ce repo doit respecter ces règles pour garantir la qualité du code et faciliter la collaboration entre les développeurs:

- <u>**SRP (Single Responsibility Principle)**</u> : Une fonction, une classe ou un module ne doit faire qu'une seule chose.

    Exemple : Une fonction load_and_clean_data() est mauvaise. Crée load_data() et clean_data().

- <u>**KISS (Keep It Simple, Stupid**</u> : Évite l'"over-engineering". La solution la plus simple à lire est souvent la meilleure. Ne crée pas des abstractions ou des hiérarchies de classes complexes si un simple script fait le travail pour le MVP.

- <u>**DRY (Don't Repeat Yourself)**</u> : Si tu copies-colles le même bloc de code plus de deux fois, extrais-le dans une fonction utilitaire partagée. Cela rend le code plus maintenable et réduit les risques d'erreurs lors de modifications futures.

- <u>**Maintenabilité**</u> : Le code doit être auto-documenté. Un nom de variable clair (is_user_authenticated) vaut infiniment mieux qu'un nom obscur (flag1) accompagné d'un commentaire. Les commentaires doivent expliquer POURQUOI on fait quelque chose, pas COMMENT (le code explique déjà le comment).

- <u>**Reproducibilité**</u> : Nous utilisons uv pour la gestion des environnements. L'utilisation directe de pip est proscrite pour éviter les dérives de versions. Tout changement de dépendance doit être validé par une mise à jour du uv.lock.

---

## 🧹 Lint & Format

* **Ruff** est notre linter et formateur unique pour Python. Il remplace flake8, isort et black en un seul outil ultra-rapide.
* Aucun commit ne doit casser le lint.

Commandes :

```bash
uv run ruff check .
uv run ruff format .
```

Vérifie le format sans modifier les fichiers :

```bash
uv run ruff format --check .
```
---

## 🌿 Git Workflow

### 🔀 Branching Strategy

* `main` → production
* `dev` → Branche de pré-production/intégration. Tous les tests doivent y passer.
* `feature/*` → nouvelles fonctionnalités
* `fix/*` → corrections de bugs sur la branche dev
* `hotfix/*` → Pour une correction d'urgence directement dérivée de main

---

### 🧾 Convention de commits

Politique de Commit
Quand commiter ?
Un commit doit être **atomique**. On dois commiter à chaque fois que l'on termine une unité logique de travail qui ne casse pas le projet. Il est interdit de faire un seul "gros commit" à la fin de la journée.

Comment commiter ? (Conventional Commits)
Chaque commit doit suivre la structure : `<type>(<scope>): <description>`

Types autorisés : `feat` (nouvelle fonctionnalité), `fix` (correction), `docs` (documentation), `chore` (configuration/maintenance), `refactor` (changement de code sans ajout de feature), `test` (ajout de tests).

### <u>**Exemple de commit parfait**</u> :

```bash
feat(auth): implémentation de la connexion par token JWT

- Ajout de la route POST /api/login dans Django
- Configuration de la durée de vie du token à 24h
- Gestion de l'erreur d'authentification avec un code 401

Resolves #42
```

## 🔍 Pull Requests

Une pull request est exigé pour fusionner de dev vers main:

1. **Titre explicite** reprenant la convention de commit
2. **Description claire** de ce qui a été fait et pourquoi
3. **Validation**: La PR doit etre approuvée par le linter et passer les tests automatisés
4. **Reviews**: Au moins une revue de code par un autre développeur est requise avant la fusion

---

## 🧪 Tests

* Les tests sont obligatoires pour les nouvelles features
* Framework : **pytest** avec pytest-django
* Fabriques de données : **factory_boy**
* Couverture mesurée avec **coverage**

Lancer les tests :

```bash
uv run pytest
uv run pytest --cov  # avec rapport de couverture
```
---

## 🔐 Sécurité et Variables d’environnement:

* Secrets : Ne JAMAIS commiter de mots de passe, clés API (OpenAI, Supabase) ou tokens dans le dépôt.
* Fichiers .env : Utilise un fichier .env local pour tes variables.
* Gitignore : S' assurer que .env est bien dans le .gitignore pour éviter les fuites accidentelles.
---

## Installation et Configuration:
L'outil uv a été choisi pour la gestion des environnements et des dépendances. Voici les étapes pour configurer votre environnement de développement :
* Initialiser le projet : uv sync

* Ajouter une dépendance : uv add <package>

* Lancer le serveur Django : uv run python manage.py runserver

* Lancer les tests : uv run pytest


## 🧠 Architecture

* Séparer clairement :

  * logique métier
  * accès aux données
  * contrôleurs

* Ne jamais mélanger logique IA et logique API

---

## 🚫 Interdictions

* ❌ Pas de code mort
* ❌ Pas de print() ni de breakpoint() en production
* ❌ Pas de duplication de code
* ❌ Pas de commit direct sur main

---

## ✅ Bonnes pratiques

* Nommer clairement les variables
* Ajouter des commentaires utiles (pas inutiles)
* Faire des commits petits et fréquents

---

## 📦 Dépendances

* Toujours vérifier la pertinence d’une dépendance
* Éviter les librairies inutiles

---

## 📣 Communication

* Toute décision technique importante doit être documentée
* Utiliser les issues GitHub pour discussion

---

## 🚀 Objectif

Construire un produit :

* scalable
* maintenable
* robuste
* professionnel

---

Merci de respecter ces règles pour garantir la qualité du projet.

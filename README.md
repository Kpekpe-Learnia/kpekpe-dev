# Kpékpé

[![Built with](https://img.shields.io/badge/Django-5.1-092E20.svg)](https://djangoproject.com)
[![Python](https://img.shields.io/badge/Python-3.13-3776AB.svg)](https://python.org)

Plateforme intelligente d'orientation éducative et professionnelle pour les jeunes du Togo et d'Afrique francophone.

## Prérequis

- [Docker](https://docs.docker.com/get-docker/)
- [uv](https://docs.astral.sh/uv/) pour le développement local hors Docker

## Démarrage rapide

```bash
docker-compose up
```

L'API est disponible sur `http://localhost:8000/`, l'admin Django sur `http://localhost:8000/admin/`, et la documentation sur `http://localhost:8001/`.

## Exécuter une commande dans le conteneur

```bash
docker-compose run --rm web ./manage.py [commande]
```

## Développement local (sans Docker)

```bash
uv sync
uv run python manage.py migrate
uv run python manage.py runserver
```

## Tests

```bash
uv run pytest
```

## Contribuer

Lis le fichier [CONTRIBUTING.md](./CONTRIBUTING.md) avant tout commit.
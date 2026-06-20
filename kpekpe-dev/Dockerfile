# Étage 1 : on construit les dépendances dans un venv isolé
FROM python:3.13-slim AS builder

RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq-dev gcc \
    && rm -rf /var/lib/apt/lists/*

# Crée un environnement virtuel et installe les paquets dedans
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Étage 2 : l'image finale, légère, sans gcc ni outils de build
FROM python:3.13-slim AS runtime

# Copie uniquement le venv depuis l'étage builder
COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

# Utilisateur non-root pour la sécurité
RUN useradd -m nonroot
USER nonroot

WORKDIR /code
COPY --chown=nonroot:nonroot . .

EXPOSE 8000

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--access-logfile", "-", "kpekpe_learnia.wsgi:application"]
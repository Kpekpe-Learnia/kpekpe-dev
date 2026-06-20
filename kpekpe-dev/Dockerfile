FROM python:3.13-slim AS builder

RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq-dev gcc \
    && rm -rf /var/lib/apt/lists/*

# Installe uv (binaire ultra-rapide, écrit en Rust)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Copie les manifestes et installe dans /opt/venv
COPY pyproject.toml uv.lock ./
RUN uv venv /opt/venv && \
    VIRTUAL_ENV=/opt/venv uv sync --frozen --no-dev --no-install-project

FROM python:3.13-slim AS runtime

COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

RUN useradd -m nonroot
USER nonroot

WORKDIR /code
COPY --chown=nonroot:nonroot . .

EXPOSE 8000
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--access-logfile", "-", "kpekpe_learnia.wsgi:application"]
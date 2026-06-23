FROM python:3.11-slim AS builder
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends build-essential libpq-dev && rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt
FROM python:3.11-slim
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends libpq5 curl && rm -rf /var/lib/apt/lists/* && addgroup --system app && adduser --system --ingroup app app
COPY --from=builder /install /usr/local
COPY . .
USER app
EXPOSE 8002
CMD ["gunicorn", "--bind", "0.0.0.0:8002", "--workers", "2", "app:app"]

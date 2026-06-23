FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt
FROM python:3.11-slim
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends curl && rm -rf /var/lib/apt/lists/* && addgroup --system app && adduser --system --ingroup app app
COPY --from=builder /install /usr/local
COPY . .
USER app
EXPOSE 8005
CMD ["gunicorn", "--bind", "0.0.0.0:8005", "--workers", "1", "--timeout", "120", "app:app"]

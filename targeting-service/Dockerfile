FROM python:3.11-slim
WORKDIR /app

# Instala dependências nativas necessárias para o Postgres rodar no Python
RUN apt-get update && apt-get install -y --no-install-recommends libpq5 curl && rm -rf /var/lib/apt/lists/*

# Copia os arquivos de dependências e instala tudo direto
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copia o código fonte
COPY . .

# Cria usuário não-root por segurança
RUN addgroup --system app && adduser --system --ingroup app app
USER app

EXPOSE 8003
CMD ["gunicorn", "--bind", "0.0.0.0:8003", "--workers", "2", "app:app"]
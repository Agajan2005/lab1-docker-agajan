# ========== Стадия 1: builder ==========
FROM python:3.12.1 AS builder

# -- Медленные операции --

# Установка системных зависимостей для сборки wheel'ов
RUN apt-get update \
    && apt-get install -y --no-install-recommends gcc \
    && rm -rf /var/lib/apt/lists/*

# Сборка wheel'ов
RUN --mount=type=bind,source=requirements.txt,target=/app/requirements.txt \
    pip wheel --no-cache-dir -r /app/requirements.txt --wheel-dir /app/wheels

# ========== Стадия 2: runtime ==========
FROM python:3.12.1-slim

# Копируем wheel'ы из builder
COPY --from=builder /app/wheels /wheels

# Устанавливаем зависимости python
RUN pip install --no-cache-dir /wheels/* \
    && rm -rf /wheels

# Копирование кода приложения
COPY app.py /app/

# Добавление пользователя и назначение его владельцем /app
RUN addgroup --system app \
    && adduser --system --group app \
    && chown -R app:app /app/

USER app

# Установка рабочей директории
WORKDIR /app

ENTRYPOINT ["uvicorn"]
CMD ["app:app", "--host", "0.0.0.0"]
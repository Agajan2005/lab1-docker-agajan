FROM python

# Копирование кода приложения
COPY . /app/

# Установка системных зависимостей для сборки зависимостей python
RUN apt-get update \
    && apt-get install -y gcc

# Установка зависимостей python
RUN pip install -r /app/requirements.txt

# Установка рабочей директории
WORKDIR /app

CMD ["uvicorn", "app:app", "--host", "0.0.0.0"]
# Этап 1: Сборка (build stage)
FROM python:3.12-alpine as builder

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем requirements.txt и устанавливаем зависимости
COPY requirements.txt ./
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Этап 2: Финальный образ (final stage)
FROM python:3.12-alpine

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем только необходимые файлы из этапа сборки
COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=builder /app /app

# Копируем остальные файлы
COPY . .

# Устанавливаем supervisor
RUN apk add --no-cache supervisor

# Копируем конфигурацию supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Команда для запуска supervisor
CMD ["/usr/bin/supervisord"]
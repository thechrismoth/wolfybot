# Используем многоэтапную сборку
FROM python:3.12.6-slim as builder

# Указываем рабочую директорию
WORKDIR /app

# Копируем и устанавливаем зависимости
COPY requirements.txt .
RUN pip install --upgrade pip && \
    pip install --user -r requirements.txt

# Копируем исходный код
COPY . .

# Финальный этап
FROM python:3.12.6-slim

# Создаем непривилегированного пользователя
RUN useradd -m myuser
USER myuser

# Указываем рабочую директорию
WORKDIR /app

# Копируем установленные зависимости из builder
COPY --from=builder /root/.local /home/myuser/.local
COPY --from=builder /app /app

# Устанавливаем supervisor
USER root
RUN apt-get update && \
    apt-get install -y supervisor && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Конфигурация supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Убедимся, что скрипты в .local доступны
ENV PATH=/home/myuser/.local/bin:$PATH

# Переключаемся обратно на непривилегированного пользователя
USER myuser

# Команда для запуска supervisor
CMD ["/usr/bin/supervisord"]

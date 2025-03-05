# Используем легкий базовый образ Nginx на основе Alpine
FROM nginx:alpine

# Копируем конфигурацию Nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Открываем порт 80
EXPOSE 80

# Команда для запуска Nginx
CMD ["nginx", "-g", "daemon off;"]
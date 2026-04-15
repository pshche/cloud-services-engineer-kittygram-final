FROM node:18 AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

# Второй этап: просто копируем файлы в volume
FROM alpine:latest

# Создаём директорию для статики
RUN mkdir -p /static

# Копируем собранные файлы напрямую в /static
COPY --from=build /app/build /static

# Если внутри static есть папка static, поднимаем содержимое
RUN if [ -d "/static/static" ]; then \
      cp -rf /static/static/* /static/ && \
      rm -rf /static/static; \
    fi

# Бесконечный цикл, чтобы контейнер не завершался
CMD ["tail", "-f", "/dev/null"]

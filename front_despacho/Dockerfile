# Etapa 1: Construcción (Node.js)
FROM node:20-alpine AS builder
WORKDIR /app

# Copiar archivos de dependencias y limpiar caché
COPY package*.json ./
RUN npm ci

# Copiar el resto del código y construir la versión de producción
COPY . .
RUN npm run build

# Etapa 2: Servidor Web (Nginx sin privilegios)
# Esta imagen corre con el usuario 'nginx' (no root) por defecto
FROM nginxinc/nginx-unprivileged:alpine

# Copiar los estáticos compilados desde la etapa anterior
# NOTA: Cambia '/app/dist' por '/app/build' si usas Create React App en lugar de Vite
COPY --from=builder /app/dist /usr/share/nginx/html

# Exponer el puerto 8080 (las imágenes non-root no pueden usar el puerto 80)
EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
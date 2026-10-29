# 1. Étape de build (compilation React pour la prod)
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# 2. Étape de dev (serveur Vite avec HMR)
FROM node:22-alpine AS dev
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]

# 3. Étape de prod non-root (Nginx unprivileged corrigé)
FROM nginxinc/nginx-unprivileged:alpine
USER root
RUN apk update && apk upgrade --no-cache brotli-libs
USER nginx
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]
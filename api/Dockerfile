FROM node:22-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

# Attribuer la propriété des fichiers à l'utilisateur node
COPY --chown=node:node . .

# Basculer sur l'utilisateur non privilégié
USER node

EXPOSE 3000

CMD ["node", "src/server.js"]
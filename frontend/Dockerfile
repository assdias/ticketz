FROM node:20-alpine as build-deps

WORKDIR /usr/src/app
COPY . .

ENV NODE_OPTIONS --openssl-legacy-provider

RUN --mount=type=cache,target=/root/.npm \
    npm ci && npm run build

# Usando o servidor HTTP do Node.js para servir os arquivos estáticos
FROM node:20-alpine

WORKDIR /usr/src/app

# Copiar os arquivos de build para o diretório correto
COPY --from=build-deps /usr/src/app/build /usr/src/app/build

# Instalar dependências necessárias para servir os arquivos
RUN npm install -g serve

# Servir os arquivos estáticos
CMD serve -s /usr/src/app/build -l 80

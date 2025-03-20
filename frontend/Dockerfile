FROM node:20-alpine as build-deps

WORKDIR /usr/src/app
COPY . .

ENV NODE_OPTIONS --openssl-legacy-provider

RUN --mount=type=cache,target=/root/.npm \
    npm ci && npm run build

FROM ghcr.io/ticketz-oss/nginx-alpine

WORKDIR /usr/share/nginx/html

COPY --from=build-deps /usr/src/app/build /var/www/public
COPY --from=build-deps /usr/src/app/node_modules/@socket.io/admin-ui/ui/dist /var/www/public/socket-admin
COPY nginx /etc/nginx

# Define a URL fixa do backend diretamente no config.json
CMD echo '{
    "BACKEND_URL": "https://services-ticketz-backend.frujoj.easypanel.host/"
}' > /var/www/public/config.json \
    && nginx -g "daemon off;"

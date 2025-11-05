FROM node:18-bullseye-slim AS builder
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci --prefer-offline --no-audit
COPY . .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
EXPOSE 80

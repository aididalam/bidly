# syntax=docker/dockerfile:1
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
COPY bidly-frontend-auth/package.json bidly-frontend-auth/package.json
COPY bidly-frontend-aunction/package.json bidly-frontend-aunction/package.json
RUN npm ci
COPY bidly-frontend-auth bidly-frontend-auth
ARG VITE_AUCTION_APP_URL=http://localhost:5174
ENV VITE_AUCTION_APP_URL=$VITE_AUCTION_APP_URL
RUN npm run build --workspace=@auction/auth-frontend
FROM nginx:1.27-alpine
COPY bidly-frontend-auth/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/bidly-frontend-auth/dist /usr/share/nginx/html
EXPOSE 80

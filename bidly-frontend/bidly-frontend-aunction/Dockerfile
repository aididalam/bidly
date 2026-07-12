# syntax=docker/dockerfile:1
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
COPY bidly-frontend-auth/package.json bidly-frontend-auth/package.json
COPY bidly-frontend-aunction/package.json bidly-frontend-aunction/package.json
RUN npm ci
COPY bidly-frontend-aunction bidly-frontend-aunction
ARG VITE_AUTH_APP_URL=http://localhost:5173
ARG VITE_S3_PUBLIC_BASE_URL=http://localhost:9000/auction-images
ENV VITE_AUTH_APP_URL=$VITE_AUTH_APP_URL
ENV VITE_S3_PUBLIC_BASE_URL=$VITE_S3_PUBLIC_BASE_URL
RUN npm run build --workspace=@auction/market-frontend
FROM nginx:1.27-alpine
COPY bidly-frontend-aunction/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/bidly-frontend-aunction/dist /usr/share/nginx/html
EXPOSE 80

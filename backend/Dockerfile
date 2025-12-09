# backend/Dockerfile
# ------------------
# Multi-stage build: install deps, build (if any), run with a small runtime image.
FROM node:20-alpine AS deps
WORKDIR /app
# Install build deps (if native modules needed)
RUN apk add --no-cache python3 make g++ \
 && npm i -g npm@latest

# copy package manifests
COPY package.json package-lock.json ./
RUN npm ci --production

# copy source
FROM node:20-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
# if you have a build step (babel/typescript), run it here:
# RUN npm run build

# create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

ENV NODE_ENV=production
EXPOSE 3000

# default command — assume server listens on process.env.PORT
CMD ["node", "index.js"]

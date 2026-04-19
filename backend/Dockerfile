FROM node:20-alpine AS build

WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm ci && npm cache clean --force

#bcaz our backend is lightweight we no need to make build
#we can directly run the app
#------production stage---------
FROM node:20-alpine AS production
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

#installing alpine Linux dependencies
RUN apk --no-cache add dumb-init:latest

WORKDIR /app

COPY --from=build /app/node_modules ./node_modules
COPY src/ ./src
USER appuser
EXPOSE 5000

ENTRYPOINT [ "dumb-init","--" ]
CMD ["node","src/index.js"]
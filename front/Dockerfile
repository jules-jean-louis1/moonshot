# 1. Install dependencies only when needed
FROM node:lts-alpine AS deps
# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache libc6-compat 

WORKDIR /app
COPY package*.json ./
RUN npm install

# 2. Rebuild the source code only when needed
FROM node:lts-alpine AS development

ENV NEXT_TELEMETRY_DISABLED=1

RUN apk add --no-cache curl

RUN mkdir -p /app && chown node:node /app

WORKDIR /app
USER node

COPY --chown=node:node --from=deps /app/node_modules ./node_modules
COPY --chown=node:node . .

RUN npm run build

EXPOSE 4000

CMD ["npm", "run", "dev"]

# 3. Production image, copy all the files and run next
FROM development AS production

CMD ["npm", "start"]

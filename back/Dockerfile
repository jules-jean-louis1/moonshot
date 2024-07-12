# Stage 1: Build the application
FROM node:18-alpine AS builder
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
RUN npm install -g @nestjs/cli
# COPY prisma ./prisma/
# RUN npx prisma generate
COPY . .
RUN npm run build

# Stage 2: Run the application
FROM node:18-alpine
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app ./
RUN npm install --production
RUN npm install -g @nestjs/cli
EXPOSE 3000
CMD ["npm", "run", "start:dev"]
# ----------- Stage 1: Builder -----------
    FROM node:18 AS builder

    WORKDIR /app

    COPY package*.json ./
    
    RUN npm install
    
    COPY . .
    
    # ----------- Stage 2: Production Image -----------

    FROM node:18-slim AS production
    
    ENV NODE_ENV=production
    ENV PORT=3003
    
    WORKDIR /app
    
    COPY --from=builder /app .
    
    RUN npm install --omit=dev
    
    EXPOSE 3003

    CMD ["node", "server.js"]
    
# ---------- BUILD STAGE ----------
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# Accept build arg and set as env var BEFORE build
ARG NEXT_PUBLIC_API_URL=http://localhost:8000
ENV NEXT_PUBLIC_API_URL=$NEXT_PUBLIC_API_URL

RUN npm run build


# ---------- RUNTIME STAGE ----------
FROM node:20-alpine

WORKDIR /app

# copy only required files from build stage
COPY --from=builder /app/package*.json ./
# Install ONLY production dependencies — no dev deps
RUN npm install --omit=dev

COPY --from=builder /app/.next ./.next

EXPOSE 3000

CMD ["npm", "start"]
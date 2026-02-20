# ---------- Stage 1: Build Dependencies ----------
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./

# Install only production dependencies
RUN npm ci --omit=dev

# Copy application source
COPY . .


# ---------- Stage 2: Production ----------
FROM node:18-alpine

WORKDIR /app

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy only production node_modules and app files
COPY --from=builder /app /app

# Change ownership to non-root user
RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 5000

CMD ["node", "server.js"]

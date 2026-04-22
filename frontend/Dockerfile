cat > frontend/Dockerfile << 'EOF'
FROM node:18-slim AS builder
WORKDIR /app
COPY package.json .
RUN npm install --production

FROM node:18-slim
WORKDIR /app
RUN useradd -m -u 1000 appuser
COPY --from=builder /app/node_modules ./node_modules
COPY . .
RUN chown -R appuser:appuser /app
USER appuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD node -e "require('http').get('http://localhost:3000', (r) => process.exit(r.statusCode === 200 ? 0 : 1))"
CMD ["node", "app.js"]
EOF
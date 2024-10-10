# 1단계: 환경설정 및 dependency 설치
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# 2단계: 빌드 단계
RUN npm run build
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package*.json ./
RUN npm install --production
ENV NEXT_PUBLIC_SERVER_API_URL=http://action-be:8080

# 3단계: 실행 단계
CMD ["npm", "run", "start"]
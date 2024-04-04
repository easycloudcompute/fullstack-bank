FROM --platform=linux/arm64/v8 node:16.14-alpine

WORKDIR /app-frontend

COPY . .

RUN npm install

RUN npm run build

CMD ["npm", "start"]

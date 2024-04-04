FROM --platform=linux/arm64/v8 node:16.14-alpine

WORKDIR /app-backend

COPY . .

RUN npm install

CMD ["npm", "start"]

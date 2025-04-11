FROM node:23-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN apk update && apk add --no-cache \
    && rm -rf /var/cache/apk/*

RUN npm install && \
    npm cache clean --force && \
    mv /usr/src/app/node_modules /node_modules

COPY . .

EXPOSE 3002

CMD ["npm", "start"]
FROM node:18-alpine

WORKDIR /nodejs

COPY package*.json ./

RUN npm install

RUN echo "$APP_PORT"

COPY . .

# EXPOSE 3000

CMD [ "npm", "start" ]


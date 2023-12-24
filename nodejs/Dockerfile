FROM node:18-alpine

ARG APP_PORT

WORKDIR /nodejs

COPY package*.json ./

RUN npm install

RUN echo "$APP_PORT"

ENV APP_PORT "$APP_PORT"

COPY . .

# EXPOSE 3000

CMD [ "npm", "start" ]


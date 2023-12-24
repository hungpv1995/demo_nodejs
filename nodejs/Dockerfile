FROM node:18-alpine

ARG BUILD_ENV

WORKDIR /nodejs

COPY package*.json ./

RUN npm install

RUN echo "$BUILD_ENV"

ENV BUILD_ENV "$BUILD_ENV"

COPY . .

# EXPOSE 3000

CMD [ "npm", "start" ]


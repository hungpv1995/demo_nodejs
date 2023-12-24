FROM node:18-alpine

WORKDIR /nodejs

COPY package*.json ./

RUN npm install

RUN echo "$BUILD_ENV"

ENV BUILD_ENV "$BUILD_ENV"

COPY . .

ENV BUILD_ENV "$BUILD_ENV"

# EXPOSE 3000

CMD [ "npm", "start" ]


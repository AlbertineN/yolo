FROM node:alpine as build

WORKDIR /usr/src/yolo

COPY package*.json ./

RUN npm ci

COPY . ./

FROM node:20.0.0-alpine3.16

COPY --from=build /usr/src/yolo /

EXPOSE 3000

CMD [ "npm", "start" ]

FROM node:20.0.0-alpine3.16 as build

WORKDIR /usr/src/yolo-back

COPY package*.json ./

RUN npm ci

COPY . ./

FROM node:20.0.0-alpine3.16

COPY --from=build /usr/src/yolo-back /

EXPOSE 5000

CMD [ "npm", "start" ]

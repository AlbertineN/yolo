FROM node:alpine

WORKDIR /usr/src/yolo-back

COPY package*.json .

RUN npm ci

COPY . .

CMD [ "npm", "start" ]
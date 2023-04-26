FROM node:alpine

WORKDIR /usr/src/yolo

COPY package*.json .

RUN npm install

COPY . .
EXPOSE 3000

ENTRYPOINT [ "npm", "start" ]
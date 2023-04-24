FROM node:8

WORKDIR /client-yolo

COPY . .

RUN npm install

EXPOSE 3000

ENTRYPOINT [ "npm", "start" ]
FROM node:8

WORKDIR /backend-yolo

COPY . .

RUN npm install

EXPOSE 5000

ENTRYPOINT [ "npm", "start" ]
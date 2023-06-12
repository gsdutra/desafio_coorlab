FROM node:16 as build-stage

WORKDIR /usr/src

COPY . .

RUN npm i

CMD ["npm", "run", "serve"]

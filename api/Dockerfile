FROM node:16

WORKDIR /app

COPY data.json .

RUN npm install -g json-server

EXPOSE 3000

CMD ["npx", "json-server", "--watch", "data.json", "--host", "0.0.0.0"]
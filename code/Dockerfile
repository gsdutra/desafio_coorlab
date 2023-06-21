FROM node:16 as build-stage

WORKDIR /app

COPY . .

RUN npm i

RUN npm run build

FROM nginx:alpine as production-stage

COPY --from=build-stage /app/dist /usr/share/nginx/html

EXPOSE 80

ENV VUE_APP_API_URL=http://localhost:3000

CMD ["nginx", "-g", "daemon off;"]
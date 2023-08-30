FROM node:16.20.2-buster AS build
WORKDIR app
COPY . .
RUN yarn install
RUN yarn build

FROM nginx:latest
COPY --from=build /app/dist/frontend /usr/share/nginx/html
EXPOSE 80
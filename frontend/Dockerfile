FROM node:16.20.2-buster
WORKDIR app
COPY . .
RUN yarn install
ENTRYPOINT ["yarn", "start"]
EXPOSE 4200
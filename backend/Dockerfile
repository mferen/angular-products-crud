FROM node:16.20.2-buster
WORKDIR app
COPY . .
RUN yarn install
RUN addgroup appgroup 
RUN adduser appuser
RUN usermod -a -G appgroup appuser
RUN chown -R appuser:appgroup /app
USER appuser
ENTRYPOINT ["yarn", "start"]
EXPOSE 3001
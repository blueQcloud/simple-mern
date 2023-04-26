# Dockerfile for React client

# Build react client
FROM node:10.16-alpine
USER 0
ENV BUILDER_VERSION 1.1

# Working directory be app
RUN mkdir /usr/app
WORKDIR /usr/app

COPY package*.json ./

###  Installing dependencies

RUN npm install

# copy local files to app folder
COPY . .

RUN npm run build

FROM caddy:2.4.5-alpine

RUN mkdir /usr/app-root
RUN mkdir /usr/app-root/etc

WORKDIR /usr/app-root/www

COPY --from=builder /usr/app/build .
COPY Caddyfile /usr/app-root/etc

RUN caddy fmt /usr/app-root/etc/Caddyfile --overwrite

RUN chmod -R 775 /opt/app-root/*
WORKDIR /opt/app-root/www

EXPOSE 3000

CMD ["/usr/bin/caddy", "run", "--config", "/usr/app-root/etc/Caddyfile"]
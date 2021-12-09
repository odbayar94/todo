FROM alpine:3.15.0 as react

WORKDIR /usr/src/react
COPY . .
RUN apk add --update nodejs npm && npm install -g yarn && yarn && yarn build

FROM nginx:1.21.4-alpine
RUN rm -rf /usr/share/nginx/html/*
COPY --from=react /usr/src/react/build /usr/share/nginx/html
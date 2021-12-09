FROM alpine:3.15.0

COPY . .
RUN apk add --update nodejs npm && npm install
CMD [ "node", "index.js" ] 
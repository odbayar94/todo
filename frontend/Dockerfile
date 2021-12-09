FROM nginx:latest
WORKDIR /usr/share/nginx/html
RUN rm -rf *
COPY public/ .
#COPY . /usr/share/nginx/html
#WORKDIR /usr/share/nginx/html
#RUN apt update -y && apt install -y nodejs npm && npm install && npm build
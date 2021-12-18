# To-Do App

Энэхүү гарын авлагын хүрээнд жижиг хэмжээний todo application хийх бөгөөд docker container дээр байршуулж, github-ийн Continuous Integration / Continuous Deployment (CI/CD) хийх юм.

### Ашиглах технологи

- Node JS
- React JS
- ExpressJS
- MongoDB
- Docker container
- GitHub, Git action

### Системийн шаардлага spec

- Node JS >= v16.0.1
- NPM >= v8.0
- Docker Engine >= 20.10.11

## Гүйцэтгэх үндсэн алхам

1. Front-end төслөө эхлүүлж, local орчинд ажлуулах.
2. MongoDB Docker container дээр үүсгэх (local орчин MongoDB суурилуулахад хүндрэлтэй байсан тул шууд docker container дээр суурилуулсан).
3. Back-end төслөө эхлүүлж, local орчинд ажлуулах.
   1. Docker image-ийг Docker Hub дээр байршуулах.
   2. Docker Hub дээрх image-ийг ашиглаж docker container ажлуулах.
4. Front-end төслөө docker container дээр суурилуулж ажлуулах.
5. Back-end төслөө docker container дээр суурилуулж ажуулах.
6. Front-end, Back-end болон MongoDB-ээ docker composer ашиглан ажлуулах.
7. Git action холбож CI/CD хэрэгжүүлэх.

## Аппликейшний ерөнхий танилцуулга

- Drag-n-Drop cards and list
- Material UI
- Delete and Add Cards and Lists
- Update/Edit List and Cards

### MongoDB docker container үүсгэх

Доорх командыг terminal дээр ажлуулж MongoDB docker container-ийг шууд ажлуулна.

```bash
docker run -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=root -p 27017:27017 --name todo-mongo mongo:latest
```

#### Командын тайлбар

- -e нь орчины тохиргоо бөгөөд MongoDB-ийн root хэрэглэгчийн нэр, нууц үгийг тохируулж байна. MONGO_INITDB_ROOT_USERNAME болон дээр тохиромжтой нэр нууц үг оруулна уу.
- -p порт expose хийж байгаа үйлдэл. 27017 нь MongoDB-ийн үндсэн порт байдаг бөгөөд энэ тохиололд үндсэн серверийн 27018 портыг **conatiner**-ийн 27017 порттой холбож байна.
- --name хэсэг нь container-ийн нэрийг онооно. Энэ хэсгийг оруулаагүй үед docker нь тухайн container-т санамсаргүй нэр оноодог.
- mongo:latest хэсэг нь hub.docker.com дээрх MongoDB-ийн сүүлийн image-ийг авч байна ажлуулж байна. latest хэсэгт та өөрт тохирох хувилбарыг дугаарыг бичих боломжтой.

### Back-end төслөө эхлүүлэх

backend хавтас руу шилжиж доорх командыг ажлуулж төслөө эхлүүлнэ.

```bash
npm install
```

```bash
node index.js
```

![backend project run](https://i.ibb.co/3RxpDWN/step4.png)

## 3. Back-end төслөө эхлүүлж, local орчинд ажлуулах

backend хавтас дотор terminal дээр доорх командыг ажлуулна.

#### Docker image үүсгэх

```bash
docker build . -t backendtodo:v1
```

#### Командын тайлбар

- **build** image үүсгэх
- **.** Dockerfile файл байгаа замыг заана. Энэ тохиолдолд Dockerfile тухайн хавтсанд байга тул зөвхөн . бичнэ.
- **-t backendtod** target image-ийн нэрийг заана.
- **v1** image-ийн tag хэрэв оруулаагүй тохиолдолд latest болно.

#### Docker container ажлуулах

Үүсгэсэн image-с docker container-ийг ажлуулна. MongoDB docker run хийхтэй адилхан тул командын тайлбарыг оруулахгүй орхилоо.

```bash
docker run --name todobackend -p 3000:3000 backendtodo:v1
```

### 3.1 Docker image-ийг Docker Hub дээр байршуулах

hub.docker.com руу нэвтрэнэ. Хэрэв танд бүртгэл байхгүй бол бүртгүүлнэ үү.
Бүртгүүлсний дараа **Create Repository** дарж repo үүсгэнэ.

![step1](https://i.ibb.co/ZcyBSrx/step1.png)

<br/>
<br/>
Repository-ийн нэрийг оруулна. Public-аар үүсгэж байгаа тохиолдолд үнэгүй байдаг. Манай жишээ нь дээр todo-backend нэртэй docker repository үүсгэсэн.
![step2](https://i.ibb.co/9n0Vg4F/step2.png)

<br/>
<br/>
Repository-г амжилттай үүсгэсэн тохиолдолд дараах хэсэг гарч ирнэ.

![step3](https://i.ibb.co/bPWsWvm/step3.png)

### 3.1 Docker image-ийг Docker Hub дээр байршуулах

Local орчинд үүсгэсэн image-ийг docker hub дээрх repository-ийн нэрээр нэрлэнэ.

```bash
docker tag odbayar94/todo-backend todo-backend:v1
```

#### Командын тайлбар

- **odbayar94/todo-backend** нэрлэх image
- **todo-backend:v1** local image-ийн нэр tag

```bash
docker push odbayar94/todo-backend
```

![push docker](https://i.ibb.co/YP1rh9V/step5.png)

### 3.2 Docker Hub дээрх image-ийг ашиглаж docker container ажлуулах

Доорх командыг terminal дээр ажлуулна. Docker hub дээрх image-ыг татаж docker container-г ажлуулна.

```bash
docker run -p 3000:3000 odbayar94/todo-backend
```

#### Командын тайлбар

- odbayar94/todo-backend энэ хэсэг нь хаанаас image татаж container-ийг ажлуулах вэ гэдгийг заана. **odbayar94** docker hub дээр таны хэрэглэгчийн нэр. **todo-backend** image-ийн нэр.

![pull and run docker](https://i.ibb.co/jWwKQK8/step6.png)

## 6. Front-end, Back-end болон MongoDB-ээ docker composer ашиглан ажлуулах

Аппликейшнд ашиглаж байгаа container-уудыг тус бүрд нь үүсгэж, асаалгүйгээр зөвхөн нэг командаар үүсгэж, ажлуулах боломжийг docker-composer олгоно. Docker Desktop суурилуулахад docker-composer хамт суудаг. Хэрэв таны компьютер дээр хараахан суугаагүй бол [docker hub](https://docs.docker.com/compose/install/)-с татаж авч суулгана уу.

```bash
dockerе-compose build
```

![docker-compose build](https://i.ibb.co/K6bpqfD/step7.png)

```bash
dockerе-compose up
```

![docker-compose up](https://i.ibb.co/MBJSRsL/step.png)

```yml
services:
  todo-fe:
    build: ./frontend
    ports:
      - "89:80"

  todo-be:
    build: ./backend
    links:
      - "todo-db"
    ports:
      - "3000:3000"

  todo-db:
    image: mongo:latest
    volumes:
      - H:\todo-db:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: root
```

### docker-compose.yml файлийн тайлбар

- **todo-fe** front-end сервисийн нэр.
- **build: ./frontend** front-end-ийг үүсгэх Dockerfile-ийн path.
- **ports: - "89:80"** local орчны 89 дугаар портыг 80 порт руу expose хийнэ.
- **todo-be:** back-end сервисийн нэр.
- **build: ./backend** back-end-ийг үүсгэх Dockerfile-ийн path.
- **links: - "todo-db"** өгөгдлийн сантай сүлжээний холболт хийнэ.
- **todo-db:** өгөгдлийн сангийн сервисийн нэр.
- **volumes: - H:\todo-db:/data/db** docker container-ыг устгаад үүсгэхэд өгөгдлийн сангийн дата давхар устдаг тул өгөгдлийн сангийн датагаа хадгалж авна. Яг тохиргоогоор docker container дахин асаахад өгөгдлийн сангийн датаг ашиглах боломжтой болно.

Front-end dockerfile-ийн бүтэц

```Dockerfile
FROM alpine:3.15.0 as react

WORKDIR /usr/src/react
COPY . .
RUN apk add --update nodejs npm && npm install -g yarn && yarn && yarn build

FROM nginx:1.21.4-alpine
RUN rm -rf /usr/share/nginx/html/*
COPY --from=react /usr/src/react/build /usr/share/nginx/html
```

### Dockerfile-ийн тайлбар

- **alpine:3.15.0** docker image ашиглана.
- **WORKDIR** ажиллах хавтасын path-ийг заана. Хэрэв image-д тус хавтас байхгүй бол шинээр үүсгэнэ.
- **COPY . .** source кодыг docker image рүү хуулна.
- **RUN apk add --update nodejs npm && npm install -g yarn && yarn && yarn build** nodejs суулгаж project-ийг build хийнэ.
- **FROM nginx:1.21.4-alpine** docker image ашиглаж, ningx сервер ажлуулна
- **RUN rm -rf /usr/share/nginx/html/\*** nignx-ийн default running path нь **/usr/share/nginx/html/** байдаг тул тус хавтас доторхыг утсгана.
- **COPY --from=react /usr/src/react/build /usr/share/nginx/html** үүсгэсэн react image дэх build хийсэн файлаа nginx-ийн running path руу бүхэлд нь хуулна.

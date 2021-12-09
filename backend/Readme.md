### Import

Create MongoDB container

```bash
docker run -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=root -p 8080:27017 --name todo-mongo mongo:latest
```

Build the bakend Docker container

```bash
docker build . -t backendtodo:v1
```

Run docker container

```bash
docker run --name todobackend -p 3000:3000 backendtodo:v1
```

Push to odbayar94/todo-backend:lates
You can run below command and execute the docker container application

```bash
docker run -p 3000:3000 odbayar94/todo-backend:lates
```

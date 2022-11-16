## EJERCICIO 1

### 1 - Creamos la red

```
docker network create lemoncode-challenge
```

### 2 - MongoDB

- Creamos el contenedor MongoDB.

```
docker run -d --name some-mongo \
    -p 27017:27017 \
    --network lemoncode-challenge \
    -v volume-some-mongo:/data/db \
    mongo:latest
```

- Añadir los datos a la base de datos como se indica en el ejercicio.

### 3 - Backend

- Ejecutamos:

```
npm run build
```

- Creamos la imagen backend.

```
docker build -t backend-challenge .
```


- Creamos el contenedor backend.

```
docker run -d --name topics-api \
    -e DATABASE_URL=mongodb://some-mongo:27017 \
    -e HOST=topics-api \
    --network lemoncode-challenge \
    backend-challenge
```

### 6 - Frontend

- Cambiamos en el fichero views/index.ejs de front `<%- topic.topicName %>` por `<%- topic.Name %>`.

- Creamos la imagen backend.

```
docker build -t frontend-challenge .
```

- Creamos el contenedor front

```
docker run -d --name frontend \
    -p 8080:3000 \
    -e API_URI=http://topics-api:5000/api/topics \
    --network lemoncode-challenge \
    frontend-challenge
```

### EJERCICIO 2

Se crea el fichero `docker-compose.yml`.

Hay que añadir los datos a la base de datos como se indica en el ejercicio.

- Levantar el entorno:

```
docker-compose up -d
```

- Parar el entorno:

```
docker-compose stop
```

- Eliminar el entorno:

```
docker-compose down -v
```
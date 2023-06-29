# App-list

# Ejecutar la aplicación con Docker

Asegúrate de tener Docker Desktop instalado en tu computadora antes de ejecutar la aplicación utilizando Docker.

## Pasos para ejecutar la aplicación

1. Abre la terminal y navega hasta la carpeta donde has extraído el archivo suministrado en el repositorio de GitHub.

2. Construye la imagen de Docker ejecutando el siguiente comando:

```
docker build -t my-app .
```

Esto construirá la imagen  Docker para la aplicación, utilizando el archivo Dockerfile presente en la carpeta. Asegúrate de incluir el punto al final del comando para indicar la ubicación actual.

3. Una vez que la construcción de la imagen se complete, ejecuta el contenedor utilizando el siguiente comando:
```
docker run -p 8080:8080 my-app
```

Esto ejecutará el contenedor utilizando la imagen que construimos previamente. El parámetro `-p` mapea el puerto 8080 del contenedor al puerto 8080 de tu máquina local.

4. Ahora puedes acceder a la aplicación abriendo tu navegador web y navegando a `http://localhost:8080`. Verás la aplicación ejecutándose en el contenedor de Docker.

Recuerda que debes asegurarte de tener Docker Desktop en ejecución antes de ejecutar estos comandos.


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

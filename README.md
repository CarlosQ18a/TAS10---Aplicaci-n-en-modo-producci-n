# Informe del Proyecto: Despliegue de Aplicación Frontend con Backend Personalizado usando Docker

## 1. Título
Integración y despliegue de una aplicación React con un backend Express personalizado utilizando Docker

## 2. Fundamentos
Este proyecto tiene como objetivo integrar un frontend desarrollado con React con un backend personalizado implementado en Express.js. Ambos servicios se despliegan en contenedores Docker, simulando un entorno de desarrollo moderno y portable.

Se aplicaron los siguientes fundamentos:

- Uso de Docker para contenerizar aplicaciones Node.js y React.
- Creación de una API REST básica con Express para representar una entidad del proyecto de titulación (evaluaciones).
- Consumo de dicha API desde un frontend React mediante peticiones HTTP (fetch o Axios).
- Exposición de puertos para permitir la comunicación entre el navegador y los servicios.
- Orquestación con Docker Compose para simplificar la ejecución conjunta.

## 3. Conocimientos Previos Requeridos
- Manejo básico de contenedores con Docker.
- Fundamentos de APIs REST con Express.js.
- Desarrollo de interfaces de usuario con React.
- Uso de Git y terminal para clonar repositorios, instalar dependencias y levantar servidores.
- Configuración de puertos y CORS para servicios locales.

## 4. Objetivos del Proyecto
- Implementar un backend Express que exponga una API REST (/api/evaluaciones).
- Crear un frontend React que consuma dicha API.
- Contenerizar el frontend mediante Docker para su despliegue en producción.
- Validar la correcta conexión entre frontend y backend localmente.
- Documentar el procedimiento y resultados obtenidos.

## 5. Equipo y Herramientas Necesarias
- Computadora con Docker y Node.js instalados.
- Visual Studio Code como editor de código.
- Navegador moderno para realizar pruebas.
- Git y GitHub para la clonación de proyectos.
- Docker Desktop o CLI para la construcción y ejecución de imágenes.

## 6. Material de Apoyo
- Documentación oficial de Docker
- Express.js
- React
- Guía de Dockerfile para React
- Axios para peticiones HTTP

## 7. Procedimiento

### Paso 1: Creación del Backend con Express

Se implementó una API REST básica en Node.js con Express:

```js
const express = require('express');
const cors = require('cors');
const app = express();
const port = 4000;

app.use(cors());

const evaluaciones = [
  { id: 1, nombre: "Evaluación de Listening", fecha: "2025-06-01", nivel: "A1", estudiante: "Carlos Quituisaca" },
  { id: 2, nombre: "Evaluación de Reading", fecha: "2025-06-02", nivel: "A2", estudiante: "Ana Pérez" },
  { id: 3, nombre: "Evaluación de Speaking", fecha: "2025-06-03", nivel: "B1", estudiante: "Luis Gómez" }
];

app.get('/api/evaluaciones', (req, res) => res.json(evaluaciones));

app.listen(port, () => {
  console.log(`✅ Backend corriendo en http://localhost:${port}`);
});
```
Se ejecuta con:
```bash
node index.js
```
### Paso 2: Creación del Frontend con React
El frontend fue desarrollado para mostrar la lista de evaluaciones consumiendo la ruta http://localhost:4000/api/evaluaciones.

```jsx
useEffect(() => {
  fetch('http://localhost:4000/api/evaluaciones')
    .then(res => res.json())
    .then(data => setEvaluaciones(data));
}, []);

```
### Paso 3: Dockerfile para el Frontend (build y NGINX)
Etapa 1: Construcción
```bash
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

```
Etapa 2: Servir con NGINX
```bash
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```
### Paso 4: Construcción de la imagen Docker del Frontend
```bash
docker build -t mi-frontend-evaluaciones .
```
### Paso 5: Ejecución del contenedor Docker del Frontend
```bash
docker run -d -p 3000:80 mi-frontend-evaluaciones
```
El frontend queda disponible en:
```bash
docker run -d -p 3000:80 mi-frontend-evaluaciones](http://localhost:3000
```
### Paso 6: Verificación de Conectividad
Se accedió a http://localhost:3000 y el frontend mostró correctamente los datos provenientes del backend Express corriendo en http://localhost:4000.

Se revisaron posibles errores relacionados con:

- Configuración de CORS en el backend.

- URLs correctas en el frontend.

- Puertos expuestos y mapeados adecuadamente.

## 8. Resultados Obtenidos
- Backend funcionando correctamente en http://localhost:4000/api/evaluaciones.

- Frontend React desplegado exitosamente en Docker, accesible en http://localhost:3000.

- Comunicación fluida entre frontend y backend.

- Contenerización facilitó el despliegue y portabilidad.

## 9. Bibliografía
- Docker Inc. (2025). Docker Documentation. Recuperado de https://docs.docker.com

- React Developers. (2025). React – A JavaScript library for building user interfaces. Recuperado de https://reactjs.org/docs/getting-started.html

- Express.js Foundation. (2025). Express – Fast, unopinionated, minimalist web framework for Node.js. Recuperado de https://expressjs.com/

- NGINX, Inc. (2025). NGINX Documentation. Recuperado de https://nginx.org/en/docs/


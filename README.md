# Tienda de Alimentos para Perritos 🐾 - Base de Datos

Este repositorio contiene la configuración e inicialización de la base de datos para la **Tienda de Alimentos para Perritos**. Define el contenedor MySQL y la estructura de tablas iniciales.

## 🚀 Tecnologías Utilizadas

- **MySQL**: Motor de base de datos relacional.
- **Docker**: Contenedorización de la base de datos para facilitar el despliegue local y en la nube.

## 📁 Estructura del Proyecto

- `db/`: Contiene el archivo de inicialización SQL (`init.sql`) y el `Dockerfile`.
  - `init.sql`: Script que crea la base de datos, la tabla `productos` e inserta algunos productos iniciales de prueba.
- `.github/workflows/db-deploy.yml`: Workflow de GitHub Actions para compilar y desplegar el contenedor de base de datos en AWS ECS Fargate.
- `.aws/`: Definición de tareas para el servicio de base de datos en AWS.

## 🛠️ Cómo Ejecutar Localmente

### Requisitos Previos

- Docker instalado.

### Pasos para iniciar la Base de Datos

1. Construye la imagen de la base de datos:
   ```bash
   docker build -t tienda-db ./db
   ```
2. Ejecuta el contenedor exponiendo el puerto `3306`:
   ```bash
   docker run -d -p 3306:3306 --name tienda-db-container tienda-db
   ```

El contenedor ejecutará el script `init.sql` automáticamente al iniciarse por primera vez, creando la base de datos `tienda` y la tabla `productos`.

## ⚙️ Integración Continua (GitHub Actions)

El repositorio cuenta con un flujo CI/CD definido en `.github/workflows/db-deploy.yml` que:
1. Se activa al realizar un `push` en la rama `main`.
2. Compila la imagen Docker de la base de datos.
3. Sube la imagen a **Amazon ECR**.
4. Despliega la nueva versión en el servicio de base de datos dentro de **Amazon ECS Fargate** (`tienda-perritos-cluster`).

# mysql-sakila

### **Instrucciones para instalar y ejecutar MySQL en Docker (Windows y Mac)**

#### **1. Instalar Docker**

Para poder ejecutar el contenedor de MySQL, es necesario tener **Docker** instalado en tu computadora.

**Windows:**

1. Descarga Docker Desktop desde [Docker Desktop para Windows](https://www.docker.com/products/docker-desktop).
2. Ejecuta el instalador y sigue las instrucciones en pantalla.
3. Una vez instalado, abre **Docker Desktop** y asegúrate de que Docker esté corriendo (debería aparecer un ícono de Docker en la barra de tareas).
4. Si se solicita habilitar **WSL 2** (Windows Subsystem for Linux), sigue las instrucciones para habilitarlo.

**Mac:**

1. Descarga Docker Desktop desde [Docker Desktop para Mac](https://www.docker.com/products/docker-desktop).
2. Ejecuta el instalador y sigue las instrucciones en pantalla.
3. Abre **Docker Desktop** y verifica que Docker está en funcionamiento (debería aparecer un ícono de Docker en la barra de menús en la parte superior de la pantalla).

#### **2. Configurar el archivo `.env` (opcional pero recomendado)**

En la misma carpeta donde se encuentre el archivo `docker-compose.yml`, debes crear un archivo llamado `.env` para definir las variables de entorno de manera segura:

```plaintext
MYSQL_ROOT_PASSWORD=tu_root_password
MYSQL_DATABASE=sakila
MYSQL_USER=user
MYSQL_PASSWORD=password
```

Esto asegurará que las contraseñas y otros parámetros no estén visibles directamente en el archivo de configuración.

#### **3. Crear el archivo `docker-compose.yml`**

En una carpeta de tu preferencia, debes crear un archivo llamado `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:latest
    container_name: sakila_mysql
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: sakila
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - ./sakila-data:/docker-entrypoint-initdb.d
```

**Explicación:**
- El archivo configura un servicio de MySQL en un contenedor.
- El contenedor se llamará `sakila_mysql`.
- Las credenciales de acceso están almacenadas en variables de entorno que se toman del archivo `.env`.
- Se exponen los puertos para acceder a MySQL desde tu máquina local.
- Se monta la carpeta `sakila-data` para inicializar la base de datos con archivos SQL.

#### **4. Ejecutar el contenedor de MySQL**

1. Abre una terminal o línea de comandos (**PowerShell** en Windows o **Terminal** en Mac).
2. Navega a la carpeta donde está el archivo `docker-compose.yml`.
3. Ejecuta el siguiente comando para iniciar el servicio de MySQL:

   ```bash
   docker-compose up -d
   ```

   Esto descargará la imagen de MySQL (si no está en la máquina) y levantará el contenedor en segundo plano.

4. Verifica que el contenedor está corriendo con el siguiente comando:

   ```bash
   docker ps
   ```

   Deberías ver el contenedor `sakila_mysql` en la lista de contenedores activos.

#### **5. Instalar MySQL Workbench**

Para interactuar con la base de datos de MySQL de forma gráfica, debes instalar **MySQL Workbench**:

- Descarga **MySQL Workbench** desde el sitio oficial: [MySQL Workbench](https://dev.mysql.com/downloads/workbench/).
- Instala siguiendo las instrucciones para Windows o Mac.

#### **6. Conectar MySQL Workbench al contenedor de MySQL**

1. Abre **MySQL Workbench**.
2. Crea una nueva conexión:
   - **Host**: `localhost`
   - **Port**: `3306` (el puerto que se expuso en el archivo `docker-compose.yml`).
   - **Username**: `user`
   - **Password**: usar la contraseña que definiste en el archivo `.env` o en el `docker-compose.yml`.

3. Prueba la conexión. Si todo está correcto, podrás conectarte al servidor MySQL que corre dentro del contenedor.

#### **7. Verificar la base de datos Sakila**

Una vez conectado en MySQL Workbench:
1. Navega al panel de la izquierda y busca la base de datos llamada **sakila**.
2. Puedes ejecutar consultas SQL y practicar sobre esta base de datos.

#### **8. Apagar o reiniciar el contenedor**

- Para apagar el contenedor (detener el servidor de MySQL), ejecuta:

   ```bash
   docker-compose down
   ```

- Para reiniciar el contenedor en otro momento, simplemente ejecuta:

   ```bash
   docker-compose up -d
   ```

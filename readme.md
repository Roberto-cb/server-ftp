# 🔐 FTSAPP_5

Servidor de Transferencia de Archivos Seguro sobre TLS + API REST + Interfaz Web

---

## 📄 Descripción

**FTSAPP_5** es una aplicación Node.js que implementa un servidor de transferencia de archivos cifrado (similar a FTPS) con:

- Un **servidor TLS personalizado** que permite listar, subir, descargar, renombrar y borrar archivos.
- Una **API REST segura** expuesta mediante HTTPS.
- Una **interfaz web** para gestionar los archivos de forma sencilla.
- Un **cliente interactivo en consola** para pruebas manuales.

Toda la comunicación se realiza de forma cifrada usando certificados TLS.

---

## 🛠️ Tecnologías Utilizadas

- **Node.js**: entorno de ejecución principal.
- **TLS/HTTPS**: cifrado de la comunicación (módulo `tls` y `https`).
- **Express.js**: servidor web y API REST.
- **Multer**: manejo de subidas de archivos.
- **CORS**: habilitar solicitudes cruzadas desde la UI.
- **JavaScript**: tanto frontend como backend.
- **HTML/CSS/JS**: interfaz web estática en `public/`.

---

## 📁 Estructura de Carpetas

FTSAPP_5/
├── node_modules/
├── public/            # Frontend web
│   ├── index.html
│   ├── main.js
│   └── style.css
├── src/               # Código fuente backend
│   ├── certs/         # Certificados TLS
│   ├── downloads/     # Descargas del cliente
│   ├── files/         # Archivos en el servidor TLS
│   ├── uploads/       # Subidas temporales
│   ├── secure-client.js
│   ├── secure-server.js
│   ├── tls-client.js
│   └── web-server.js
├── package.json
├── package-lock.json
└── README.txt

---

## 🚀 Pasos de Ejecución

A continuación, se detallan los pasos completos para ejecutar el proyecto:

### 1️⃣ Clonar el repositorio

git clone <URL_DEL_REPOSITORIO>
cd FTSAPP_5

---

### 2️⃣ Instalar dependencias

npm install

---

### 3️⃣ Generar certificados TLS (si no existen)

Puedes generar certificados autofirmados usando OpenSSL:

cd src/certs

# Crear clave privada
openssl genrsa -out server-key.pem 2048

# Crear certificado autofirmado válido por 1 año
openssl req -new -x509 -key server-key.pem -out server-cert.pem -days 365

cd ../..

Si ya tienes certificados, cópialos en src/certs/:

- server-key.pem
- server-cert.pem

---

### 4️⃣ Iniciar el servidor TLS de archivos

Este proceso escucha en el puerto 6000 y gestiona los comandos (GET, PUT, LIST, etc.):

node src/secure-server.js

Verás en consola:

[SERVER] Servidor TLS escuchando en puerto 6000

---

### 5️⃣ (Opcional) Usar el cliente interactivo en consola

Puedes interactuar manualmente con el servidor TLS:

En una consola ejecuta lo siguiente:
node src/secure-server.js
La consola devulve lo siguiente: 
[SERVER] Servidor TLS escuchando en puerto 6000

Y en otra consola ejecuta lo siguiente:
node src/secure-client.js
Te duvuelve esto: 
Comandos: LIST, GET <file>, PUT <file>, DELETE <file>, RENAME <old> <new>, SALIR
Comando>

Paso por paso como de como usar las funciones del servidor.
a) LIST — listar archivos en servidor
En la consola escribe

LIST
El servidor debe responder con la lista de archivos dentro de la carpeta files/ (por ejemplo: OK: FILES test.txt).

b) GET <file> — descargar un archivo
Escribí:
GET test.txt
El cliente recibirá el archivo y lo guardará en la carpeta downloads/.

En la terminal verás mensajes de progreso y al final:

[CLIENT][GET] Descarga completa
Revisá que downloads/test.txt se haya creado y contenga el mismo contenido que files/test.txt.

c) PUT <file> — subir un archivo al servidor
Antes que nada, asegurate que el archivo que vas a subir exista en la carpeta uploads/.

Por ejemplo, si subís uploads/archivoParaSubir.txt, en el cliente escribí:
PUT archivoParaSubir.txt
El cliente esperará confirmación del servidor, enviará el archivo y mostrará algo como:

[CLIENT][PUT] Servidor listo. Enviando archivo...
[CLIENT][PUT] Archivo enviado, cerrando escritura...
[CLIENT][PUT] Respuesta: OK: PUT complete
Revisá que el archivo aparezca en la carpeta files/ del servidor con el mismo contenido.

d) DELETE <file> — borrar archivo en servidor
Ejemplo:

DELETE archivoParaSubir.txt
El servidor eliminará el archivo de files/.

El cliente mostrará:

[CLIENT][RESPONSE] OK: DELETE complete

e) RENAME <old> <new> — renombrar archivo en servidor
Ejemplo:

RENAME test.txt nuevoNombre.txt
El servidor renombrará el archivo dentro de files/.

El cliente verá:

[CLIENT][RESPONSE] OK: RENAME complete

f) SALIR — cerrar cliente
Para salir:

SALIR
El cliente cerrará la conexión y terminará.


Los archivos descargados se guardan en src/downloads/ y los que subas deben estar en src/uploads/.

---

### 6️⃣ Iniciar el servidor web HTTPS (API REST + UI)
Este es el servidor que gestiona archivos.

node src/secure-server.js

Salida esperada:
[SERVER] Servidor TLS escuchando en puerto 6000


En otro terminal:

node src/web-server.js

Esto expondrá la API y la interfaz web en:

https://localhost:3000

Al abrir esa URL en el navegador, podrás:

- Listar archivos
- Descargar archivos
- Subir archivos
- Borrar y renombrar

Nota: al ser un certificado autofirmado, el navegador mostrará advertencia de seguridad. Puedes aceptarla para continuar.

Estos son los paso a seguir para testear la api

Primero, en su pagina principal.


![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/f127585ee5a68a2a8bdfd8e1cfa845ff191e7919/Screenshot_10.png)


2) Para subir un archivo, tendra que darle el boton que dice seleccionar archivo:
![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/e17841ff1c27024957774547deddb61701f306b9/Screenshot_10.png)

Buscara en su escritorio cualquier archivo que quiera subir, en mi caso sera una foto de queen.
![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/b7ca58ee07dcbce59050d9ebe95d3873145a758f/Screenshot_2.png)


3)Luego de subir el archivo debera aparecerle el nombre del archivo y con los botones de descargar, renombrar y eliminar.
![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/1bea814b9e57842c9d3523cb4932e31160a29161/Screenshot_3.png)


4) Vamos a testear la funcionalidad de renombrar.
   Vamos a precionar el boton azul de renombrar y le saldra lo siguiente.
   ![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/585a92c1940443f0de7ebd9f6bb4fa39654c40b9/Screenshot_4.png)
   Dentro de ese input pondra el nuevo nombre del archivo, en mi caso deci escribir el nombre de la banda queen.png
   Luego de darle acpertar le saldra lo suguiente.
   
   ![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/585a92c1940443f0de7ebd9f6bb4fa39654c40b9/Screenshot_5.png)

   Una confirmacion que muestrara por cual hemos cambiado el nombre al archivo, damos en aceptar.
   Asegurece de que en la pagina principal le salga el archivo con el nuevo nombre, tal que asi:
 
 ![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/72edd78ff1353acbaeec8ba2453332ae21c7b043/Screenshot_6.png)

5)Descargar:
Solo precione el boton de descargar y se bajara el archivo sin ningun problema:
![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/af61a3d2e2eef4311a692d12a37554bd412faae5/Screenshot_7.png)


6)Eliminar:
En este paso, para testear que se elimne el archivo, solo presionaremos en el boton azul eliminar y nos saldra lo siguiente:
![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/3b24aa1c20b545f8d2ca90b884df7bb653c30f4b/Screenshot_8.png)
Nos preguntara si queremos eliminar el archio que hemos seleccionado. 
Daremos en aceptar para continuar con la eliminacion.
Y nos devolvera una notificacion que hemos eliminado el archivo exitosamente
![Image Alt](https://github.com/Roberto-cb/server-ftp/blob/a429974900d1cc0f4ef9b5560cfd01f7e894e153/Screenshot_9.png)
Recargue la pagina y asegurese que no aparezca el archivo.



   




## 🌐 Endpoints de la API REST

| Método | Ruta                         | Descripción                       |
|--------|------------------------------|-----------------------------------|
| GET    | /api/files                   | Lista todos los archivos         |
| GET    | /api/files/:filename         | Descarga un archivo              |
| POST   | /api/files                   | Sube un archivo                  |
| DELETE | /api/files/:filename         | Elimina un archivo               |
| PUT    | /api/files/:oldName          | Renombra un archivo              |

---





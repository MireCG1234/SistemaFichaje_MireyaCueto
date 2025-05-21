# SistemaFichaje_MireyaCueto
Proyecto prácticas de empresa 1ºDAM.

**Sistema de Fichajes** es una aplicación de escritorio desarrollada en Java, diseñada para gestionar el registro de entrada y salida de los trabajadores de una empresa. La aplicación está pensada para funcionar en sistemas operativos **Windows 10**, con una **interfaz gráfica sencilla e intuitiva** desarrollada con JavaSwing y con conexión a una base de datos MySQL alojada en Docker.

---

## Características principales

- Registro de **entrada y salida** de los trabajadores mediante un código exclusivo de 4 cifras.
- **Mensajes de respuesta** según el código ingresado:
  - `El código no existe / Error en el código insertado`, cuando pones un código que no pertenece a ningún trabajador o que no tiene 4 cifras.
  - `¡Bienvenid@!`, cuando un trabajador pone su código e ingresa a la empresa.
  - `¡Hasta la próxima!`, cuando un trabajador pone su código y se marcha de la empresa.

![errorImgae](https://github.com/user-attachments/assets/b55b7d60-382a-4cf5-84c2-7aa53e15a4f3)
![bienvenida](https://github.com/user-attachments/assets/41ce7a4c-8178-4088-b3f8-ddae5938ca98)
![salida](https://github.com/user-attachments/assets/1c255289-373c-4830-aa75-d9ee20a4ff82)


- Interfaz gráfica con:
  - Logo de empresa (en este caso MiPC, ya que es la empresa asignada para las prácticas).
  - Teclado numérico en pantalla junto a teclas para borrar, ir hacia atrás e ingresar el código.
  - Fecha y hora actual en pantalla que se actualiza.
- Acceso de administrador mediante código especial que permite visualizar:
  - **Listado completo de fichajes** de todos los trabajadores en el siguiente formato:
    ```
    NombreTrabajador - CódigoTrabajador - Entrada/Salida - Fecha - Hora
    ```

---

## Interfaz de usuario

La ventana principal de la aplicación incluye:
- Logo de empresa en la parte superior derecha.
- Campo para mostrar mensajes de error, bienvenida o despedida.
- Campo para ingresar el código de fichaje.
- Teclado numérico funcional.
- Fecha y hora actualizadas en tiempo real.

![imagenMenu1](https://github.com/user-attachments/assets/8b39c157-498f-4163-a8b1-70b46fde9bba)

La ventana administrador incluye:
- Logo de empresa en la parte superior centro.
- Botón de volver al menú principal.
- Listado de registros ordenados de más recientes a más antiguos.

![imagenMenu2](https://github.com/user-attachments/assets/8bdd50c7-2a0f-48b1-afd2-6347818c51ac)


---

## Requisitos del sistema

- **Sistema operativo:** Windows 10 / Windows 11
- **Java:** JDK 8 o superior
- **Base de datos:** Local, integrada en el mismo equipo (en este caso MySQL local)
- **Acceso directo en el escritorio** para facilitar la ejecución del programa

---

## Base de datos

- Contiene una tabla de **USERS** (usuarios) con sus datos y códigos asignados previamente.
- Cada Usuario cuenta con:
  - CODE INTEGER(4) PRIMARY KEY : Codigo 4 dígitos.
  - NAME VARCHAR(25) : Nombre de usuario.
  - ISADMIN BOOLEAN : Tipo de usuario (trabajador/admin, que se representa con una booleana).
  - LAST_ENTRY_TYPE BOOLEAN : Tipo de fichaje que le toca a la próxima (entrada/salida, representado por una booleana, 1=entra/0=sale).

- Además, contiene una tabla de **SIGNINGS** (registros/fichajes) que identifica cuándo un usuario ha fichado.
- Con cada fichaje se almacena:
  - CODE INTEGER(4) FOREIGN KEY : Código del trabajador
  - ENTRY_TYPE BOOLEAN : Tipo de fichaje (entrada/salida)
  - ENTRY_DATE TIMESTAMP : Fecha y hora del fichaje en ese instante

![baseDatos](https://github.com/user-attachments/assets/ffa62ffe-2082-4596-a651-1c1d65c9e641)


---

## Instalación y ejecución

1. Clonar este repositorio: git clone https://github.com/tuusuario/control-fichaje-java.git
2. Importar el proyecto en tu IDE IntelliJ (Opcional: Si quieres ver el proyecto desde dentro).

![intellijEditor](https://github.com/user-attachments/assets/86d664ef-e2e8-497f-a501-d5af4d85c13c)


3. Configurar la base de datos localmente:
   - Descargar e instalar mysql y phpmyadmin en un contenedor de docker (también puede ser en local).
   - Crear mediante phpmyadmin la base de datos con los requisitos mostrados anteriormente, o ejecutar directamente el siguiente script en la consola SQL:

drop database if exists signing_system_db;
create database signing_system_db;
use signing_system_db;

CREATE TABLE USERS (
	CODE INTEGER(4) NOT NULL,
	NAME VARCHAR(25),
	ISADMIN BOOLEAN DEFAULT NULL,
	LAST_ENTRY_TYPE BOOLEAN DEFAULT FALSE,
	CONSTRAINT PK_USERS primary key(CODE)
);

INSERT INTO USERS VALUES (1111,"Carlos",TRUE,TRUE);
INSERT INTO USERS VALUES (2222,"Loli",TRUE,TRUE);
INSERT INTO USERS VALUES (3333,"Antonio",FALSE,TRUE);
INSERT INTO USERS VALUES (4444,"Esteban",FALSE,TRUE);
INSERT INTO USERS VALUES (5555,"Lorena",FALSE,TRUE);
INSERT INTO USERS VALUES (6666,"Luis",FALSE,TRUE);

CREATE TABLE SIGNINGS (
	CODE INTEGER NOT NULL,
	ENTRY_DATE TIMESTAMP,
	ENTRY_TYPE BOOLEAN DEFAULT NULL,
	CONSTRAINT FK_SIGNINGS FOREIGN KEY (CODE) 
		REFERENCES USERS(CODE)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);

     
5. Ejecutar el archivo .jar exportado desde un acceso directo en el escritorio.

![bat](https://github.com/user-attachments/assets/e44fab61-7bed-4159-a1cd-60f969e2ff04)



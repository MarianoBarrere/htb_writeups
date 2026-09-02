Informe de Resolución: Máquina "Sequel" (HTB)

Autor: TTTO
Fecha: 2 de septiembre de 2026

1. Resumen Ejecutivo

En este informe documento mi proceso para comprometer la máquina "Sequel" de Hack The Box. Durante la evaluación, identifiqué que el servidor exponía públicamente su servicio de base de datos MariaDB con una configuración insegura. Esto me permitió acceder con privilegios máximos sin necesidad de autenticación y extraer la flag objetivo.

2. Reconocimiento y Escaneo

Comencé la evaluación realizando un escaneo de puertos y servicios sobre la IP del objetivo para descubrir su superficie de ataque. Para esto, ejecuté en mi terminal el comando nmap -sV -sC -p- <IP_OBJETIVO> con el fin de buscar versiones, lanzar los scripts básicos y analizar todos los puertos disponibles. El resultado de este escaneo me permitió identificar que únicamente el puerto 3306/TCP se encontraba abierto, el cual estaba ejecutando el servicio de base de datos MySQL/MariaDB.

3. Explotación y Acceso

Al observar este servicio expuesto, mi primer paso fue comprobar si existían credenciales por defecto o nulas, lo cual es una falla común en entornos mal configurados. Para intentar autenticarme remotamente como el usuario administrador sin proporcionar ninguna contraseña, utilicé el cliente estándar y ejecuté el comando mysql -h <IP_OBJETIVO> -u root.

El servidor aceptó mi conexión inmediatamente y me dio acceso a la consola de la base de datos. Una vez adentro, comencé a enumerar la información ejecutando la sentencia SHOW DATABASES; para ver qué bases de datos existían. Al identificar una de interés, ingresé a ella mediante la sentencia USE htb;. Posteriormente, listé sus tablas internas ejecutando SHOW TABLES;. Finalmente, al localizar la tabla que contenía la información, extraje todo su contenido con la sentencia SELECT * FROM config;.


4. Conclusión y Remediación

Logré comprometer la máquina debido a una falla crítica de configuración: el usuario administrativo de MariaDB no tenía contraseña asignada y el servicio permitía conexiones desde cualquier IP externa.

Para asegurar este servidor y evitar futuras intrusiones, recomiendo dos acciones principales. Primero, establecer una contraseña robusta para el usuario root y ejecutar el script mysql_secure_installation para eliminar accesos anónimos. Segundo, restringir el acceso de red modificando el archivo de configuración de MariaDB (usualmente my.cnf) para establecer el parámetro bind-address = 127.0.0.1. Esto asegurará que el puerto 3306 solo acepte conexiones locales, bloqueando de manera efectiva cualquier intento de acceso remoto.

1. Resumen Ejecutivo

   
Durante la evaluación de seguridad realizada al servidor objetivo (Fawn), se identificó una vulnerabilidad de riesgo medio/alto vinculada a una mala configuración en el servicio de transferencia de archivos. El sistema permite el acceso a usuarios anónimos sin requerir autenticación, lo que posibilita a cualquier atacante externo enumerar, leer y descargar archivos internos del servidor, comprometiendo la confidencialidad de la información.

2. Hallazgos Técnicos (Metodología de Ataque)


    Reconocimiento y Enumeración: Se ejecutó un escaneo de puertos sobre la dirección IP del objetivo utilizando nmap. Los resultados revelaron que el servidor tenía expuesto el puerto 21/TCP.

    Análisis de Servicios: La inspección del puerto 21 confirmó la ejecución del protocolo FTP (File Transfer Protocol).

    Explotación (PoC - Prueba de Concepto): Se procedió a interactuar con el servicio utilizando el cliente nativo de FTP (ftp <IP>). Al solicitar credenciales, se comprobó la habilitación del inicio de sesión anónimo ingresando el usuario anonymous con una contraseña en blanco.

    Impacto: El servidor aceptó la conexión, otorgando acceso al directorio de archivos. Esto permitió navegar por la estructura de carpetas (comandos ls, cd) y exfiltrar el archivo de validación (flag) hacia la máquina del atacante utilizando el comando get.

3. Remediación y Recomendaciones

   
    Para mitigar este vector de ataque y proteger la confidencialidad de los datos, se recomienda aplicar las siguientes acciones:

_Deshabilitar el acceso anónimo: Modificar el archivo de configuración del servidor FTP (por ejemplo, vsftpd.conf o proftpd.conf) para asegurar que la directiva anonymous_enable esté configurada en NO.

_Autenticación y Cifrado: Exigir credenciales robustas para todos los usuarios y, de ser posible, transicionar de FTP tradicional (texto plano) a protocolos seguros como SFTP (basado en SSH) o FTPS (FTP sobre SSL/TLS) para cifrar los datos en tránsito.

_Aislamiento de Directorios (Chroot Jail): Configurar el servidor para que los usuarios autenticados queden "encerrados" únicamente en su directorio personal, evitando que puedan navegar hacia archivos críticos del sistema operativo.

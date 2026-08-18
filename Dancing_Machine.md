1. Resumen Ejecutivo:


    Durante la evaluación de seguridad realizada al servidor objetivo (Dancing), se identificó una vulnerabilidad de riesgo medio/alto relacionada con la configuración del servicio de uso compartido de archivos en entornos Windows (SMB). El servidor permite la enumeración y el acceso a recursos compartidos utilizando sesiones nulas (credenciales en blanco). Esto permite a un atacante externo navegar por directorios internos y exfiltrar información confidencial sin necesidad de evadir mecanismos de autenticación.

3. Hallazgos Técnicos (Metodología de Ataque):


    Reconocimiento y Enumeración: Se ejecutó un escaneo de puertos sobre la IP del objetivo utilizando nmap. Los resultados revelaron la exposición del puerto 445/TCP.

    Análisis de Servicios: El puerto 445 confirmó la ejecución del servicio SMB (Microsoft-DS).

    Explotación (PoC - Prueba de Concepto): Se utilizó la herramienta smbclient para intentar una conexión de sesión nula o de invitado. Mediante el comando de enumeración (smbclient -L //<IP>/ -U ''), se listaron los recursos compartidos disponibles, revelando una carpeta accesible llamada "WorkShares".

    Impacto: El sistema permitió la conexión directa al recurso compartido (smbclient //<IP>/WorkShares -U ''). Una vez dentro, se logró navegar por la estructura de directorios y descargar el archivo de validación (flag) hacia la máquina del atacante utilizando el comando get.

4. Remediación y Recomendaciones:
   
    Para mitigar este vector de ataque y proteger la red corporativa, se recomienda aplicar las siguientes acciones al equipo de infraestructura:

    Deshabilitar el acceso de invitado/anónimo: Configurar las políticas locales de Windows o el registro (parámetro RestrictAnonymous) para bloquear la enumeración de sesiones nulas y exigir autenticación válida para cualquier intento de conexión a SMB.
    Control de Acceso (Principio de Menor Privilegio): Revisar los permisos de la carpeta "WorkShares" asegurando que solo los usuarios y grupos de red explícitamente autorizados tengan acceso de lectura/escritura.

    Higiene de Red (Firewall): El puerto 445 (SMB) jamás debe estar expuesto a Internet. Se debe configurar el firewall perimetral para bloquear el tráfico entrante al puerto 445, permitiéndolo únicamente dentro de segmentos de red locales seguros o a través de una VPN.

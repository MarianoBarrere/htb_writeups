1. Resumen Ejecutivo
    
    Durante la evaluación de seguridad realizada al servidor objetivo (Meow), se identificó una vulnerabilidad de riesgo crítico vinculada a malas prácticas de configuración y uso de protocolos obsoletos. Un atacante externo podría obtener control total del sistema (acceso administrativo supremo) en cuestión de segundos y sin necesidad de evadir sistemas de autenticación complejos, lo que representa un riesgo inminente para la confidencialidad e integridad del entorno.

2. Hallazgos Técnicos (Metodología de Ataque)

    Reconocimiento y Enumeración: Se inició la auditoría ejecutando un escaneo de puertos sobre la dirección IP del objetivo utilizando nmap. El escaneo reveló que el servidor tenía expuesto el puerto 23/TCP.
    Análisis de Servicios: Al inspeccionar el puerto 23, se confirmó la ejecución del servicio Telnet, un protocolo heredado de administración remota conocido por transmitir datos en texto plano y carecer de cifrado.
    Explotación (PoC - Prueba de Concepto): Se procedió a interactuar manualmente con el servicio utilizando el cliente nativo de Telnet en Linux (telnet <IP> 23). El servidor presentó un login prompt. Se comprobó la existencia de credenciales por defecto/nulas ingresando el usuario administrativo root sin proveer contraseña.
    Impacto: El sistema otorgó acceso directo a una terminal con privilegios máximos (root shell), permitiendo la lectura del archivo de validación (flag) y comprometiendo el servidor en su totalidad.

3. Remediación y Recomendaciones Para mitigar este vector de ataque, se recomienda aplicar las siguientes acciones al equipo de infraestructura:

    Deshabilitar Telnet inmediatamente: Reemplazar el uso de Telnet por protocolos seguros y cifrados como SSH (puerto 22) para la administración remota.
    Auditoría de Credenciales: Deshabilitar los inicios de sesión sin contraseña y aplicar políticas de contraseñas robustas para todas las cuentas del sistema, prestando especial atención a los usuarios privilegiados (root, admin).
    Higiene de Red: Configurar el firewall perimetral o las reglas locales (iptables) para bloquear el acceso público a puertos de administración, restringiéndolo únicamente a las direcciones IP de los administradores autorizados.

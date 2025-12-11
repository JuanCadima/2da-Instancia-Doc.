Proyecto: Defensa Perimetral de Aplicaciones Web con WAF y Bloqueo de Intrusiones

Asignatura: SIS313: Infraestructura, Plataformas Tecnológicas y Redes

Semestre: 2/2025

Docente: Ing. Marcelo Quispe Ortega

**Miembros del Equipo (Grupo 5)**

|**Nombre Completo**|**Rol en el Proyecto**|
| :- | :- |
|Aceituno Cadima Juan Jesus|WAF|
|Michel Condori Cristian Mauricio|Nginx-SSL|
|Arando Ramo Brayan Rodrigo|Fail2ban|
|Carvajal Alcaraz Jarel|Servidor Web|


**I. Objetivo del Proyecto**

**Objetivo:** Diseñar, implementar y configurar una arquitectura de defensa perimetral para aplicaciones web mediante la implementación de un Web Application Firewall (WAF) con ModSecurity, sistema de bloqueo automático de intrusiones (Fail2ban) y monitoreo centralizado de logs, para proteger aplicaciones web vulnerables de ataques comunes como XSS, SQL Injection, RFI/LFI, y Command Injection.

**II. Justificación e Importancia**

**Justificación:** En un entorno universitario o empresarial donde se despliegan aplicaciones web con diferentes niveles de seguridad (como DVWA para prácticas), es crítico implementar mecanismos de protección que:

- **Mitiguen ataques automatizados** contra aplicaciones expuestas en Internet
- **Protejan contra vulnerabilidades conocidas** en aplicaciones legadas o en desarrollo
- **Centralicen la monitorización** de intentos de intrusión
- **Automaticen la respuesta** ante patrones de ataque repetitivos
- **Separen responsabilidades** entre servidores de aplicación y defensa perimetral

Este proyecto resuelve problemas de **seguridad (T5)** aplicando defensa en profundidad y **continuidad operacional (T1)** mediante la protección proactiva de servicios críticos.

**III. Tecnologías y Conceptos Implementados**

**3.1. Tecnologías Clave**

|Tecnología|Función Específica|
| :- | :- |
|**Nginx + ModSecurity**|Proxy inverso con WAF integrado para filtrado de tráfico HTTP/HTTPS y detección de ataques web|
|**Fail2ban**|Sistema de bloqueo automático de IPs basado en análisis de logs y patrones de ataque|
|**DVWA (Damn Vulnerable Web App)**|Aplicación web vulnerable utilizada como "cebo" para probar el WAF|
|**Apache + PHP + MariaDB**|Stack LAMP para hosting de aplicaciones web vulnerables|
|**Frontail**|Visualizador web de logs en tiempo real para monitoreo centralizado|
|**SSH + Rsync**|Transferencia segura de logs entre servidores para análisis centralizado|
|**VSFTPD**|Servidor FTP configurado para demostrar protección contra ataques de fuerza bruta|

**3.2. Conceptos de la Asignatura Puestos en Práctica**

**Seguridad y Hardening (T5):**

- Implementación de WAF con reglas OWASP CRS
- Configuración de Fail2ban con jails personalizados
- Hardening de servicios (Apache, Nginx, VSFTPD)
- Segmentación de red lógica entre servidor de aplicación y WAF
- Configuración de permisos de archivos y usuarios

**Automatización y Gestión (T6):**

- Scripts de instalación automatizada de componentes
- Configuración automática de reglas WAF
- Sincronización centralizada de logs
- Automatización de respuestas a intrusiones

**Balanceo de Carga/Proxy (T3/T4):**

- Nginx como proxy inverso con reescritura de headers
- Distribución de tráfico entre puertos (80 con WAF vs 8080 sin WAF)
- Health checks implícitos mediante monitoreo de logs

**Monitoreo (T4/T1):**

- Frontail para visualización en tiempo real de logs
- Centralización de logs de múltiples servicios
- Alertas visuales de ataques bloqueados
- Dashboard unificado de seguridad

**Networking Avanzado (T3):**

- Configuración de proxy con headers personalizados
- Redirección de puertos y virtual hosting
- Comunicación segura SSH entre servidores
- Manejo de tráfico HTTP/HTTPS

**IV. Diseño de la Infraestructura y Topología**

**4.1. Diseño Esquemático**

![](Aspose.Words.3e3a2a4c-e46b-47f4-9aeb-4717793a7894.001.png)

**4.2. Estrategia Adoptada**

**Arquitectura de Defensa en Profundidad:**

1. **Capa 1 - Aplicación Web Vulnerable:** DVWA y aplicación custom como objetivos
1. **Capa 2 - WAF (ModSecurity):** Filtrado de payloads maliciosos
1. **Capa 3 - Fail2ban:** Bloqueo de IPs tras múltiples intentos
1. **Capa 4 - Monitoreo:** Visualización en tiempo real y análisis forense

**Separación de Responsabilidades:**

- VM1 exclusiva para hosting (sin protección integrada)
- VM2 exclusiva para seguridad (aislamiento de funciones)

**Comparativa Controlada:**

- Puerto 80: Con WAF activo (producción)
- Puerto 8080: Sin WAF (pruebas/desarrollo)

**V. Guía de Implementación y Puesta en Marcha**

**5.1. Pre-requisitos**

- 2 VMs con Ubuntu 22.04/Debian 12
- Acceso sudo/root en ambas máquinas
- Conectividad de red entre VMs
- 2GB RAM mínimo por VM
- 20GB disco por VM

**5.2. Despliegue**

![](Aspose.Words.3e3a2a4c-e46b-47f4-9aeb-4717793a7894.002.jpeg)

![](Aspose.Words.3e3a2a4c-e46b-47f4-9aeb-4717793a7894.003.jpeg)

![](Aspose.Words.3e3a2a4c-e46b-47f4-9aeb-4717793a7894.004.jpeg)

**VI. Pruebas y Validación**

|Prueba Realizada|Resultado Esperado|Resultado Obtenido|
| :- | :- | :- |
|**Ataque XSS** (<script>alert(1)</script>)|Bloqueo inmediato (403)|OK - Bloqueado por ModSecurity|
|**SQL Injection** (' OR 1=1 --)|Bloqueo y log de alerta|OK - Detectado por CRS Rule 942100|
|**Path Traversal** (../../etc/passwd)|Bloqueo y posible ban de IP|OK - Bloqueado por CRS Rule 930100|
|**Command Injection** (;cat /etc/shadow)|Bloqueo y ban automático|OK - Fail2ban activado tras 5 intentos|
|**Fuerza Bruta FTP** (múltiples logins)|Bloqueo de IP atacante|OK - Fail2ban vsftpd jail activo|
|**Acceso sin WAF** (puerto 8080)|Ataques exitosos|OK - Se comprueba efectividad WAF|
|**Monitoreo Frontail**|Logs en tiempo real|OK - Dashboard operativo en :9001|
|**Desbanneo manual**|Restauración acceso|OK - fail2ban-client unbanip funcional|

**VII. Conclusiones y Lecciones Aprendidas**

**Logros Principales:**

1. **Arquitectura Efectiva:** Se demostró la eficacia de separar capas de seguridad en servidores distintos
1. **Protección en Tiempo Real:** ModSecurity + Fail2ban bloquearon el 100% de ataques de prueba
1. **Monitorización Centralizada:** Frontail permitió visualización inmediata de amenazas
1. **Comparativa Clara:** La diferencia puerto 80 vs 8080 evidenció el valor del WAF

**Desafíos Superados:**

1. **Configuración ModSecurity:** Ajuste fino de reglas CRS para balancear seguridad/funcionalidad
1. **Sincronización de Logs:** Implementación de transferencia SSH segura entre servidores
1. **Falsos Positivos:** Ajuste de reglas para aplicaciones específicas (DVWA)

**Lecciones Aprendidas:**

1. **La seguridad por capas es fundamental** - Ninguna solución única protege contra todos los vectores
1. **El monitoreo es tan importante como la prevención** - Sin visibilidad, no hay seguridad efectiva
1. **La automatización de respuestas** (Fail2ban) reduce significativamente la ventana de exposición
1. **Las pruebas comparativas** (con/sin WAF) son cruciales para validar la efectividad

**Mejoras Futuras:**

1. Implementar SSL/TLS para tráfico cifrado
1. Añadir sistema de SIEM para correlación avanzada de eventos
1. Implementar honeypots para atraer y estudiar atacantes
1. Automatizar despliegue completo con Ansible
1. Añender dashboard Grafana con métricas de seguridad

**Impacto en el Entorno Académico:**

Este proyecto proporciona un laboratorio vivo para que estudiantes:

- Experimenten ataques reales en ambiente controlado
- Comprendan el funcionamiento de WAFs empresariales
- Aprendan análisis forense mediante logs
- Desarrollen habilidades en seguridad ofensiva/defensiva

**Conclusión Final:** La implementación de defensa perimetral con WAF y Fail2ban demostró ser una solución efectiva, escalable y educativa para proteger aplicaciones web en entornos académicos y empresariales, aplicando directamente los conceptos avanzados de la asignatura en un escenario realista y práctico.


Proyecto: Defensa Perimetral de Aplicaciones Web con WAF y Bloqueo de
Intrusiones

Asignatura: SIS313: Infraestructura, Plataformas Tecnológicas y Redes

Semestre: 2/2025

Docente: Ing. Marcelo Quispe Ortega

Miembros del Equipo (Grupo 5)

I. Objetivo del Proyecto

Objetivo: Diseñar, implementar y configurar una arquitectura de defensa
perimetral para aplicaciones web mediante la implementación de un Web
Application Firewall (WAF) con ModSecurity, sistema de bloqueo
automático de intrusiones (Fail2ban) y monitoreo centralizado de logs,
para proteger aplicaciones web vulnerables de ataques comunes como XSS,
SQL Injection, RFI/LFI, y Command Injection.

II. Justificación e Importancia

Justificación: En un entorno universitario o empresarial donde se
despliegan aplicaciones web con diferentes niveles de seguridad (como
DVWA para prácticas), es crítico implementar mecanismos de protección
que:

Mitiguen ataques automatizados contra aplicaciones expuestas en Internet

Protejan contra vulnerabilidades conocidas en aplicaciones legadas o en
desarrollo

Centralicen la monitorización de intentos de intrusión

Automaticen la respuesta ante patrones de ataque repetitivos

Separen responsabilidades entre servidores de aplicación y defensa
perimetral

Este proyecto resuelve problemas de seguridad (T5) aplicando defensa en
profundidad y continuidad operacional (T1) mediante la protección
proactiva de servicios críticos.

III. Tecnologías y Conceptos Implementados

3.1. Tecnologías Clave

3.2. Conceptos de la Asignatura Puestos en Práctica

Seguridad y Hardening (T5):

Implementación de WAF con reglas OWASP CRS

Configuración de Fail2ban con jails personalizados

Hardening de servicios (Apache, Nginx, VSFTPD)

Segmentación de red lógica entre servidor de aplicación y WAF

Configuración de permisos de archivos y usuarios

Automatización y Gestión (T6):

Scripts de instalación automatizada de componentes

Configuración automática de reglas WAF

Sincronización centralizada de logs

Automatización de respuestas a intrusiones

Balanceo de Carga/Proxy (T3/T4):

Nginx como proxy inverso con reescritura de headers

Distribución de tráfico entre puertos (80 con WAF vs 8080 sin WAF)

Health checks implícitos mediante monitoreo de logs

Monitoreo (T4/T1):

Frontail para visualización en tiempo real de logs

Centralización de logs de múltiples servicios

Alertas visuales de ataques bloqueados

Dashboard unificado de seguridad

Networking Avanzado (T3):

Configuración de proxy con headers personalizados

Redirección de puertos y virtual hosting

Comunicación segura SSH entre servidores

Manejo de tráfico HTTP/HTTPS

IV. Diseño de la Infraestructura y Topología

4.1. Diseño Esquemático

4.2. Estrategia Adoptada

Arquitectura de Defensa en Profundidad:

Capa 1 - Aplicación Web Vulnerable: DVWA y aplicación custom como
objetivos

Capa 2 - WAF (ModSecurity): Filtrado de payloads maliciosos

Capa 3 - Fail2ban: Bloqueo de IPs tras múltiples intentos

Capa 4 - Monitoreo: Visualización en tiempo real y análisis forense

Separación de Responsabilidades:

VM1 exclusiva para hosting (sin protección integrada)

VM2 exclusiva para seguridad (aislamiento de funciones)

Comparativa Controlada:

Puerto 80: Con WAF activo (producción)

Puerto 8080: Sin WAF (pruebas/desarrollo)

V. Guía de Implementación y Puesta en Marcha

5.1. Pre-requisitos

2 VMs con Ubuntu 22.04/Debian 12

Acceso sudo/root en ambas máquinas

Conectividad de red entre VMs

2GB RAM mínimo por VM

20GB disco por VM

5.2. Despliegue

VI. Pruebas y Validación

VII. Conclusiones y Lecciones Aprendidas

Logros Principales:

Arquitectura Efectiva: Se demostró la eficacia de separar capas de
seguridad en servidores distintos

Protección en Tiempo Real: ModSecurity + Fail2ban bloquearon el 100% de
ataques de prueba

Monitorización Centralizada: Frontail permitió visualización inmediata
de amenazas

Comparativa Clara: La diferencia puerto 80 vs 8080 evidenció el valor
del WAF

Desafíos Superados:

Configuración ModSecurity: Ajuste fino de reglas CRS para balancear
seguridad/funcionalidad

Sincronización de Logs: Implementación de transferencia SSH segura entre
servidores

Falsos Positivos: Ajuste de reglas para aplicaciones específicas (DVWA)

Lecciones Aprendidas:

La seguridad por capas es fundamental - Ninguna solución única protege
contra todos los vectores

El monitoreo es tan importante como la prevención - Sin visibilidad, no
hay seguridad efectiva

La automatización de respuestas (Fail2ban) reduce significativamente la
ventana de exposición

Las pruebas comparativas (con/sin WAF) son cruciales para validar la
efectividad

Mejoras Futuras:

Implementar SSL/TLS para tráfico cifrado

Añadir sistema de SIEM para correlación avanzada de eventos

Implementar honeypots para atraer y estudiar atacantes

Automatizar despliegue completo con Ansible

Añender dashboard Grafana con métricas de seguridad

Impacto en el Entorno Académico:

Este proyecto proporciona un laboratorio vivo para que estudiantes:

Experimenten ataques reales en ambiente controlado

Comprendan el funcionamiento de WAFs empresariales

Aprendan análisis forense mediante logs

Desarrollen habilidades en seguridad ofensiva/defensiva

Conclusión Final: La implementación de defensa perimetral con WAF y
Fail2ban demostró ser una solución efectiva, escalable y educativa para
proteger aplicaciones web en entornos académicos y empresariales,
aplicando directamente los conceptos avanzados de la asignatura en un
escenario realista y práctico.

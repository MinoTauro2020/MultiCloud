# Análisis de Servicios de Identidad y Seguridad en Google Cloud Platform (GCP)

Este documento detalla los principales servicios de Google Cloud Platform (GCP) relacionados con la gestión de identidades, seguridad y protección de infraestructura: **Cloud Identity**, **Cloud IAM**, **Identity-Aware Proxy (IAP)**, **Threat Detection**, **Security Command Center (SCC)**, **Infrastructure Protection**, **Cloud Firewall** y **Cloud Armor**.

---

## 🛡️ Cloud Identity

**Propósito:**  
Cloud Identity es el servicio de gestión de identidades de GCP que proporciona autenticación, autorización y administración de usuarios y dispositivos para aplicaciones y recursos en la nube y entornos híbridos.

**Tipos de Logs:**  
- **Registros de inicio de sesión:** Capturan intentos de autenticación, incluyendo usuario, aplicación, dispositivo, ubicación y resultado (éxito o fallo).  
- **Registros de auditoría:** Detallan acciones administrativas, como creación de usuarios, cambios en grupos o configuraciones de dispositivos.  
- **Registros de dispositivos:** Registran actividades de dispositivos gestionados, como cumplimiento de políticas de seguridad.  

**Almacenamiento:**  
- Almacenados en **Cloud Audit Logs**, accesibles vía **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub** para análisis a largo plazo.  

**Casos de Uso:**  
- Monitorear inicios de sesión sospechosos desde dispositivos no gestionados.  
- Auditar cambios en la configuración de usuarios para garantizar cumplimiento.  

**Costo:**  
- Incluido en Cloud Identity Free para funciones básicas; funciones avanzadas requieren Cloud Identity Premium (costo por usuario/mes).  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery para logs exportados.  

---

## 🔐 Cloud IAM

**Propósito:**  
Cloud IAM (Identity and Access Management) permite a los administradores gestionar permisos de acceso a recursos de GCP con control granular, asegurando el principio de privilegio mínimo.

**Tipos de Logs:**  
- **Registros de auditoría de administración:** Capturan cambios en políticas IAM, como asignaciones o eliminaciones de roles.  
- **Registros de acceso a datos:** Detallan accesos a recursos protegidos por IAM, incluyendo usuario, recurso y acción.  
- **Registros de recomendaciones:** Generados por IAM Recommender, sugieren ajustes para reducir permisos excesivos.  

**Almacenamiento:**  
- Almacenados en **Cloud Audit Logs**, accesibles vía **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Investigar quién modificó una política IAM para un proyecto crítico.  
- Reducir permisos innecesarios basándose en recomendaciones de IAM Recommender.  

**Costo:**  
- Sin costo adicional para funciones básicas de IAM.  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery para logs exportados.  

---

## 🚦 Identity-Aware Proxy (IAP)

**Propósito:**  
IAP implementa acceso de confianza cero (zero-trust) para aplicaciones y máquinas virtuales en GCP, autenticando usuarios y verificando contexto sin necesidad de VPN.

**Tipos de Logs:**  
- **Registros de autenticación:** Capturan intentos de acceso a recursos protegidos por IAP, incluyendo usuario, resultado y contexto (e.g., IP, dispositivo).  
- **Registros de auditoría:** Detallan cambios en configuraciones de IAP, como ajustes en políticas de acceso.  

**Almacenamiento:**  
- Almacenados en **Cloud Audit Logs**, accesibles vía **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Analizar accesos denegados a una aplicación protegida por IAP debido a contexto no autorizado.  
- Configurar acceso seguro a VMs sin direcciones IP públicas mediante túneles TCP.  

**Costo:**  
- Sin costo adicional para IAP; incluido en servicios como Compute Engine, Cloud Run o App Engine.  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery para logs exportados.  

---

## 🔍 Threat Detection

**Propósito:**  
Threat Detection, parte de Security Command Center, identifica amenazas en tiempo real, como credenciales filtradas, criptominería, malware o actividad anómala, utilizando inteligencia de amenazas de Google y Mandiant.

**Tipos de Logs:**  
- **Registros de detecciones de amenazas:** Capturan eventos sospechosos, como accesos no autorizados o ejecución de malware en VMs.  
- **Registros de anomalías:** Detallan comportamientos inusuales, como tráfico de red anómalo o actividad de botnets.  
- **Registros de auditoría:** Capturan acciones relacionadas con la configuración de Threat Detection.  

**Almacenamiento:**  
- Almacenados en **Security Command Center** y **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Detectar ejecución de software de criptominería en una instancia de Compute Engine.  
- Investigar credenciales filtradas utilizadas en un intento de acceso.  

**Costo:**  
- Incluido en Security Command Center Premium o Enterprise; Standard no incluye detección avanzada.  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery para logs exportados.  

---

## 🕵️ Security Command Center (SCC)

**Propósito:**  
Security Command Center (SCC) es una solución CNAPP (Cloud-Native Application Protection Platform) que ofrece visibilidad, detección de amenazas y gestión de riesgos para entornos GCP, AWS y Azure.

**Tipos de Logs:**  
- **Registros de hallazgos de seguridad:** Capturan vulnerabilidades, configuraciones erróneas y amenazas activas (e.g., puertos abiertos, malware).  
- **Registros de activos:** Detallan inventarios de recursos y cambios en configuraciones.  
- **Registros de auditoría:** Capturan cambios en configuraciones de SCC.  
- **Registros de amenazas de Mandiant:** Incluyen inteligencia de amenazas para entornos multinube (Enterprise tier).  

**Almacenamiento:**  
- Almacenados en **Security Command Center** y **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Identificar buckets de Cloud Storage públicos que puedan exponer datos sensibles.  
- Correlacionar amenazas multinube para detectar ataques coordinados.  

**Costo:**  
- Standard tier gratuito con funciones básicas; Premium y Enterprise tienen costos basados en recursos monitoreados y funciones avanzadas.  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery.  

---

## 🏗️ Infrastructure Protection

**Propósito:**  
Infrastructure Protection, parte de Security Command Center y otras herramientas de GCP, protege recursos como VMs, redes y almacenamiento mediante monitoreo, recomendaciones y controles de seguridad.

**Tipos de Logs:**  
- **Registros de alertas:** Capturan amenazas específicas a la infraestructura (e.g., configuraciones inseguras).  
- **Registros de recomendaciones:** Detallan sugerencias para mejorar la seguridad (e.g., cerrar puertos innecesarios).  
- **Registros de auditoría:** Capturan cambios en configuraciones de seguridad de infraestructura.  

**Almacenamiento:**  
- Almacenados en **Security Command Center** y **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Detectar VMs con reglas de firewall demasiado permisivas.  
- Implementar recomendaciones para cumplir con estándares como ISO/IEC 27001.  

**Costo:**  
- Incluido en Security Command Center (Standard, Premium o Enterprise).  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery.  

---

## 🔥 Cloud Firewall

**Propósito:**  
Cloud Firewall controla el tráfico de red en VPCs, permitiendo o denegando conexiones según reglas basadas en IP, puertos y protocolos.

**Tipos de Logs:**  
- **Registros de firewall:** Capturan tráfico permitido o denegado, incluyendo IP de origen/destino, puertos y reglas aplicadas.  
- **Registros de auditoría:** Detallan cambios en reglas de firewall.  

**Almacenamiento:**  
- Almacenados en **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Analizar tráfico denegado para ajustar reglas de firewall.  
- Auditar cambios en reglas para garantizar cumplimiento.  

**Costo:**  
- Sin costo adicional para reglas de firewall básicas; Firewall Plus tiene costos por funciones avanzadas.  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery.  

---

## 🛑 Cloud Armor

**Propósito:**  
Cloud Armor protege aplicaciones contra ataques DDoS y amenazas web (e.g., inyección SQL, XSS) mediante políticas de seguridad en el borde de la red, integrándose con HTTP(S) Load Balancing.

**Tipos de Logs:**  
- **Registros de políticas:** Capturan solicitudes bloqueadas o permitidas, incluyendo IP de origen, tipo de ataque y regla activada.  
- **Registros de diagnóstico:** Detallan el estado y rendimiento de Cloud Armor.  

**Almacenamiento:**  
- Almacenados en **Cloud Logging**.  
- Exportables a **Cloud Storage**, **BigQuery** o **Pub/Sub**.  

**Casos de Uso:**  
- Mitigar un ataque DDoS contra una aplicación web.  
- Analizar solicitudes bloqueadas para ajustar políticas de seguridad.  

**Costo:**  
- Basado en el número de políticas, solicitudes procesadas y datos inspeccionados.  
- Costos por ingesta/almacenamiento en Cloud Logging, Storage o BigQuery.  

---

## 📋 Tabla Resumen

| Servicio               | Propósito                                           | Tipos de Logs                                          | Almacenamiento                          | Ejemplo de Uso                                  |
|------------------------|----------------------------------------------------|-------------------------------------------------------|----------------------------------------|------------------------------------------------|
| Cloud Identity         | Gestión de identidades y dispositivos               | Inicio de sesión, Auditoría, Dispositivos            | Cloud Logging, Storage, BigQuery, Pub/Sub | Monitorear inicios de sesión sospechosos        |
| Cloud IAM              | Gestión de permisos de acceso                      | Auditoría de administración, Acceso a datos, Recomendaciones | Cloud Logging, Storage, BigQuery, Pub/Sub | Investigar cambios en políticas IAM             |
| Identity-Aware Proxy   | Acceso de confianza cero                          | Autenticación, Auditoría                             | Cloud Logging, Storage, BigQuery, Pub/Sub | Analizar accesos denegados por contexto         |
| Threat Detection       | Detección de amenazas en tiempo real               | Detecciones de amenazas, Anomalías, Auditoría         | SCC, Cloud Logging, Storage, BigQuery, Pub/Sub | Detectar criptominería en VMs                   |
| Security Command Center| Visibilidad y gestión de riesgos                   | Hallazgos de seguridad, Activos, Auditoría, Amenazas | SCC, Cloud Logging, Storage, BigQuery, Pub/Sub | Identificar buckets públicos                    |
| Infrastructure Protection | Protección de infraestructura                   | Alertas, Recomendaciones, Auditoría                  | SCC, Cloud Logging, Storage, BigQuery, Pub/Sub | Detectar reglas de firewall permisivas          |
| Cloud Firewall         | Control de tráfico de red                         | Firewall, Auditoría                                  | Cloud Logging, Storage, BigQuery, Pub/Sub | Analizar tráfico denegado                      |
| Cloud Armor            | Protección contra DDoS y amenazas web              | Políticas, Diagnóstico                               | Cloud Logging, Storage, BigQuery, Pub/Sub | Mitigar ataques DDoS                           |

---

## 📝 Notas Clave

**Cloud Identity vs. Cloud IAM:**  
- Cloud Identity gestiona usuarios y dispositivos, proporcionando autenticación y SSO. Cloud IAM controla permisos de acceso a recursos de GCP con políticas granulares.

**Threat Detection vs. Security Command Center:**  
- Threat Detection se enfoca en identificar amenazas en tiempo real (e.g., malware, credenciales filtradas). SCC ofrece una visión integral, incluyendo vulnerabilidades, configuraciones erróneas y amenazas multinube.

**Cloud Firewall vs. Cloud Armor:**  
- Cloud Firewall protege el tráfico de red a nivel de VPC con reglas basadas en IP y puertos. Cloud Armor protege aplicaciones web contra ataques DDoS y amenazas de capa 7.

**Logs y Cloud Logging:**  
- Todos los servicios integran logs con Cloud Logging, permitiendo análisis centralizado. La exportación a Cloud Storage o BigQuery es ideal para retención a largo plazo y análisis avanzado.

**Costos:**  
- Servicios como Cloud Identity (Premium), SCC (Premium/Enterprise) y Cloud Armor tienen costos específicos. Cloud IAM y funciones básicas de Cloud Firewall son gratuitas, pero la ingesta/almacenamiento de logs genera costos.

---

## ✅ Mejores Prácticas

**Cloud Identity:**  
- Habilita MFA y políticas de dispositivos para todos los usuarios.  
- Exporta logs de auditoría a BigQuery para análisis de cumplimiento.  

**Cloud IAM:**  
- Usa IAM Recommender para identificar y eliminar permisos excesivos.  
- Implementa políticas condicionales basadas en contexto (e.g., IP, hora).  

**Identity-Aware Proxy:**  
- Configura IAP para todas las aplicaciones y VMs sin IPs públicas.  
- Monitorea logs de autenticación para detectar accesos no autorizados.  

**Threat Detection:**  
- Habilita en SCC Premium o Enterprise para detección avanzada.  
- Integra con herramientas SIEM externas vía Pub/Sub para correlación de amenazas.  

**Security Command Center:**  
- Activa la tier Premium o Enterprise para soporte multinube y Mandiant Threat Intelligence.  
- Revisa regularmente los hallazgos de seguridad y actúa sobre recomendaciones.  

**Infrastructure Protection:**  
- Implementa todas las recomendaciones de SCC antes de habilitar recursos críticos.  
- Usa Organization Policies para establecer barreras de seguridad.  

**Cloud Firewall:**  
- Define reglas granulares y revisa logs para optimizar políticas.  
- Habilita Firewall Plus para funciones avanzadas como detección de intrusos.  

**Cloud Armor:**  
- Configura políticas para mitigar ataques comunes (e.g., OWASP Top 10).  
- Monitorea logs para ajustar reglas y reducir falsos positivos.  

**General:**  
- Centraliza logs en Cloud Logging para monitoreo en tiempo real.  
- Configura alertas en Cloud Monitoring para eventos críticos.  
- Optimiza costos con políticas de retención en Cloud Storage.  

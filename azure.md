# Análisis de Servicios de Identidad y Seguridad en Azure

Este documento detalla los principales servicios de Azure relacionados con la gestión de identidades, seguridad y protección de infraestructura: **Azure Active Directory (Azure AD)**, **Privileged Identity Management (PIM)**, **Conditional Access**, **Identity Protection**, **Threat Detection**, **Azure Sentinel**, **Azure Defender**, **Infrastructure Protection**, **Web Application Firewall (WAF)**, **DDoS Protection** y **Azure Firewall Manager**.

---

## 🛡️ Azure Active Directory (Azure AD)

**Propósito:**  
Azure AD es el servicio de gestión de identidades y acceso basado en la nube de Microsoft. Proporciona autenticación, autorización y gestión de identidades para aplicaciones y recursos en Azure y entornos híbridos.

**Tipos de Logs:**  
- **Registros de inicio de sesión:** Detallan los intentos de autenticación, incluyendo usuario, aplicación, ubicación, dispositivo y resultado (éxito o fallo).  
- **Registros de auditoría:** Capturan acciones administrativas, como cambios en roles, políticas o configuraciones de usuarios.  
- **Registros de aprovisionamiento:** Registran actividades de sincronización de identidades entre Azure AD y sistemas externos (e.g., on-premises AD).  
- **Registros de aplicaciones:** Detallan el uso de aplicaciones integradas con Azure AD.

**Almacenamiento:**  
- Almacenados en Azure AD Logs, accesibles vía Azure Monitor.  
- Exportables a **Azure Monitor Logs**, **Azure Storage** o **Event Hubs** para análisis a largo plazo.  

**Casos de Uso:**  
- Investigar intentos de inicio de sesión sospechosos desde ubicaciones inusuales.  
- Auditar cambios en permisos de usuarios administrativos.  

**Costo:**  
- Registros de auditoría e inicio de sesión gratuitos por 7 días.  
- Retención extendida y exportación requieren licencias de Azure AD Premium (P1 o P2) y costos por ingesta/almacenamiento en Azure Monitor o Storage.  

---

## 🔐 Privileged Identity Management (PIM)

**Propósito:**  
PIM gestiona, controla y monitorea el acceso privilegiado en Azure AD y recursos de Azure, permitiendo activaciones temporales de roles con aprobación.

**Tipos de Logs:**  
- **Registros de auditoría de PIM:** Capturan eventos como activaciones, desactivaciones, solicitudes y aprobaciones de roles privilegiados.  
- **Registros de alertas:** Detallan alertas de seguridad, como activaciones de roles fuera de políticas.  

**Almacenamiento:**  
- Integrado con Azure AD Logs, accesible vía Azure Monitor.  
- Exportable a **Azure Monitor Logs**, **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Rastrear quién activó un rol de administrador global y cuándo.  
- Revisar solicitudes de acceso privilegiado denegadas por falta de aprobación.  

**Costo:**  
- Requiere Azure AD Premium P2.  
- Costos asociados a ingesta/almacenamiento en Azure Monitor o Storage para logs exportados.  

---

## 🚦 Conditional Access

**Propósito:**  
Conditional Access permite definir políticas basadas en condiciones (ubicación, dispositivo, riesgo) para controlar el acceso a recursos protegidos por Azure AD.

**Tipos de Logs:**  
- **Registros de inicio de sesión con Conditional Access:** Detallan evaluaciones de políticas, condiciones aplicadas y resultados (permitido, bloqueado, MFA requerido).  
- **Registros de auditoría:** Capturan cambios en políticas de acceso condicional.  

**Almacenamiento:**  
- Integrado con Azure AD Logs (inicio de sesión y auditoría).  
- Exportable a **Azure Monitor Logs**, **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Investigar por qué un usuario fue bloqueado por una política de acceso condicional.  
- Auditar modificaciones en políticas para garantizar cumplimiento.  

**Costo:**  
- Requiere Azure AD Premium P1 o P2.  
- Costos por ingesta/almacenamiento si se exportan logs a Azure Monitor o Storage.  

---

## 🛡️ Identity Protection

**Propósito:**  
Identity Protection utiliza aprendizaje automático para detectar riesgos de identidad, como inicios de sesión sospechosos o credenciales comprometidas, y permite respuestas automatizadas.

**Tipos de Logs:**  
- **Registros de detecciones de riesgo:** Capturan eventos de riesgo (e.g., inicios de sesión desde ubicaciones desconocidas, comportamiento anómalo).  
- **Registros de usuarios en riesgo:** Detallan usuarios con riesgos identificados y acciones tomadas (e.g., restablecimiento de contraseña).  
- **Registros de auditoría:** Capturan cambios en configuraciones de Identity Protection.  

**Almacenamiento:**  
- Integrado con Azure AD Logs, accesible vía Azure Monitor.  
- Exportable a **Azure Monitor Logs**, **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Identificar usuarios con credenciales potencialmente comprometidas.  
- Revisar riesgos detectados durante inicios de sesión para ajustar políticas.  

**Costo:**  
- Requiere Azure AD Premium P2.  
- Costos por ingesta/almacenamiento para logs exportados.  

---

## 🔍 Threat Detection (Azure AD)

**Propósito:**  
Threat Detection en Azure AD identifica actividades sospechosas, como ataques de fuerza bruta o inicios de sesión desde direcciones IP maliciosas, integrándose con Identity Protection y otras herramientas.

**Tipos de Logs:**  
- **Registros de eventos de seguridad:** Capturan detecciones de amenazas, como intentos de autenticación anómalos o patrones de ataque.  
- **Registros de auditoría:** Detallan acciones tomadas en respuesta a amenazas (e.g., bloqueo de IP).  

**Almacenamiento:**  
- Integrado con Azure AD Logs, accesible vía Azure Monitor.  
- Exportable a **Azure Monitor Logs**, **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Detectar y responder a ataques de fuerza bruta contra cuentas de Azure AD.  
- Analizar patrones de amenazas para fortalecer políticas de seguridad.  

**Costo:**  
- Incluido en Azure AD Premium P2.  
- Costos por ingesta/almacenamiento si se exportan logs.  

---

## 🕵️ Azure Sentinel

**Propósito:**  
Azure Sentinel es una solución SIEM (Security Information and Event Management) que proporciona análisis de seguridad, detección de amenazas y respuesta a incidentes mediante inteligencia artificial.

**Tipos de Logs:**  
- **Registros de eventos de seguridad:** Agregan logs de Azure AD, Azure Monitor, y otras fuentes (e.g., firewalls, aplicaciones).  
- **Registros de alertas:** Detallan alertas generadas por reglas de detección.  
- **Registros de incidentes:** Capturan investigaciones y respuestas a incidentes de seguridad.  
- **Registros de consultas analíticas:** Detallan consultas ejecutadas para análisis de amenazas.  

**Almacenamiento:**  
- Almacenados en **Azure Monitor Logs** (Log Analytics).  
- Opcionalmente exportables a **Azure Storage** para retención a largo plazo.  

**Casos de Uso:**  
- Correlacionar logs de múltiples fuentes para detectar ataques coordinados.  
- Investigar incidentes de seguridad mediante tableros y consultas analíticas.  

**Costo:**  
- Basado en ingesta de datos y almacenamiento en Log Analytics.  
- Costos adicionales por consultas analíticas y retención extendida.  

---

## 🛡️ Azure Defender

**Propósito:**  
Azure Defender (parte de Microsoft Defender for Cloud) proporciona protección contra amenazas para cargas de trabajo en la nube, incluyendo servidores, bases de datos, contenedores y aplicaciones.

**Tipos de Logs:**  
- **Registros de alertas de seguridad:** Detallan amenazas detectadas, como intentos de explotación o accesos no autorizados.  
- **Registros de recomendaciones:** Capturan sugerencias para mejorar la postura de seguridad.  
- **Registros de eventos de recursos:** Detallan actividades en recursos protegidos (e.g., máquinas virtuales, bases de datos).  

**Almacenamiento:**  
- Integrado con **Azure Monitor Logs** (Log Analytics).  
- Exportable a **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Detectar intentos de acceso no autorizado a una base de datos SQL en Azure.  
- Implementar recomendaciones para mitigar vulnerabilidades en servidores.  

**Costo:**  
- Basado en el tipo y cantidad de recursos protegidos.  
- Costos por ingesta/almacenamiento en Log Analytics para logs exportados.  

---

## 🏗️ Infrastructure Protection

**Propósito:**  
Infrastructure Protection (parte de Microsoft Defender for Cloud) protege recursos de Azure, como máquinas virtuales, redes y almacenamiento, mediante monitoreo y recomendaciones de seguridad.

**Tipos de Logs:**  
- **Registros de alertas:** Capturan amenazas específicas a la infraestructura (e.g., configuraciones inseguras).  
- **Registros de recomendaciones:** Detallan acciones para mejorar la seguridad de la infraestructura.  
- **Registros de auditoría:** Capturan cambios en configuraciones de seguridad.  

**Almacenamiento:**  
- Integrado con **Azure Monitor Logs** (Log Analytics).  
- Exportable a **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Identificar máquinas virtuales con puertos expuestos innecesariamente.  
- Auditar configuraciones de red para cumplir con estándares de seguridad.  

**Costo:**  
- Incluido en Microsoft Defender for Cloud (planes estándar).  
- Costos por ingesta/almacenamiento en Log Analytics.  

---

## 🌐 Web Application Firewall (WAF)

**Propósito:**  
Azure WAF protege aplicaciones web contra amenazas comunes, como inyecciones SQL, cross-site scripting (XSS) y otros ataques OWASP, integrándose con Azure Application Gateway o Azure Front Door.

**Tipos de Logs:**  
- **Registros de WAF:** Capturan solicitudes bloqueadas o permitidas, incluyendo detalles como IP de origen, tipo de ataque y regla activada.  
- **Registros de diagnóstico:** Detallan el estado y rendimiento del WAF.  

**Almacenamiento:**  
- Almacenados en **Azure Monitor Logs** (Log Analytics).  
- Exportables a **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Analizar solicitudes bloqueadas por intentos de inyección SQL.  
- Ajustar reglas de WAF para reducir falsos positivos.  

**Costo:**  
- Basado en el uso de Application Gateway o Front Door.  
- Costos por ingesta/almacenamiento en Log Analytics.  

---

## 🛑 DDoS Protection

**Propósito:**  
Azure DDoS Protection protege aplicaciones y recursos contra ataques de denegación de servicio distribuido (DDoS), mitigando tráfico malicioso.

**Tipos de Logs:**  
- **Registros de mitigación:** Capturan detalles de ataques DDoS detectados y mitigados, incluyendo volumen de tráfico y origen.  
- **Registros de diagnóstico:** Detallan el estado de la protección DDoS.  

**Almacenamiento:**  
- Almacenados en **Azure Monitor Logs** (Log Analytics).  
- Exportables a **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Analizar un ataque DDoS para identificar patrones de tráfico malicioso.  
- Verificar la eficacia de las medidas de mitigación durante un ataque.  

**Costo:**  
- DDoS Protection Standard tiene un costo fijo mensual más costos por datos procesados durante ataques.  
- Costos por ingesta/almacenamiento en Log Analytics.  

---

## 🔥 Azure Firewall Manager

**Propósito:**  
Azure Firewall Manager centraliza la gestión de políticas de seguridad para firewalls de Azure y redes virtuales, proporcionando control y monitoreo de tráfico.

**Tipos de Logs:**  
- **Registros de firewall:** Capturan tráfico permitido o denegado, incluyendo IP, puertos y reglas aplicadas.  
- **Registros de auditoría:** Detallan cambios en políticas de firewall.  
- **Registros de diagnóstico:** Capturan métricas de rendimiento y estado del firewall.  

**Almacenamiento:**  
- Almacenados en **Azure Monitor Logs** (Log Analytics).  
- Exportables a **Azure Storage** o **Event Hubs**.  

**Casos de Uso:**  
- Investigar tráfico denegado por una regla de firewall específica.  
- Auditar cambios en políticas de firewall para garantizar cumplimiento.  

**Costo:**  
- Basado en el uso de Azure Firewall y el volumen de datos procesados.  
- Costos por ingesta/almacenamiento en Log Analytics.  

---

## 📋 Tabla Resumen

| Servicio                | Propósito                                           | Tipos de Logs                                          | Almacenamiento                          | Ejemplo de Uso                                  |
|-------------------------|----------------------------------------------------|-------------------------------------------------------|----------------------------------------|------------------------------------------------|
| Azure AD                | Gestión de identidades y acceso                    | Inicio de sesión, Auditoría, Aprovisionamiento, Aplicaciones | Azure Monitor Logs, Storage, Event Hubs | Investigar inicios de sesión sospechosos        |
| PIM                     | Gestión de acceso privilegiado                    | Auditoría de PIM, Alertas                            | Azure Monitor Logs, Storage, Event Hubs | Rastrear activaciones de roles privilegiados    |
| Conditional Access      | Políticas de acceso basadas en condiciones        | Inicio de sesión, Auditoría                          | Azure Monitor Logs, Storage, Event Hubs | Investigar bloqueos por políticas               |
| Identity Protection     | Detección de riesgos de identidad                 | Detecciones de riesgo, Usuarios en riesgo, Auditoría | Azure Monitor Logs, Storage, Event Hubs | Identificar credenciales comprometidas          |
| Threat Detection        | Identificación de actividades sospechosas         | Eventos de seguridad, Auditoría                      | Azure Monitor Logs, Storage, Event Hubs | Detectar ataques de fuerza bruta                |
| Azure Sentinel          | SIEM para análisis y respuesta a incidentes       | Eventos de seguridad, Alertas, Incidentes, Consultas | Azure Monitor Logs, Storage            | Correlacionar logs para detectar ataques        |
| Azure Defender          | Protección de cargas de trabajo en la nube        | Alertas de seguridad, Recomendaciones, Eventos       | Azure Monitor Logs, Storage, Event Hubs | Detectar accesos no autorizados a bases de datos|
| Infrastructure Protection| Protección de infraestructura de Azure            | Alertas, Recomendaciones, Auditoría                  | Azure Monitor Logs, Storage, Event Hubs | Identificar puertos expuestos en VMs            |
| WAF                     | Protección de aplicaciones web                    | Registros de WAF, Diagnóstico                        | Azure Monitor Logs, Storage, Event Hubs | Analizar solicitudes bloqueadas por ataques     |
| DDoS Protection         | Mitigación de ataques DDoS                        | Mitigación, Diagnóstico                              | Azure Monitor Logs, Storage, Event Hubs | Analizar ataques DDoS                           |
| Azure Firewall Manager  | Gestión centralizada de firewalls                 | Firewall, Auditoría, Diagnóstico                     | Azure Monitor Logs, Storage, Event Hubs | Investigar tráfico denegado por reglas          |

---

## 📝 Notas Clave

**Azure AD vs. Azure Sentinel:**  
- Azure AD proporciona logs de identidad y acceso, ideales para auditorías de autenticación y autorización. Azure Sentinel agrega y analiza logs de múltiples fuentes para detectar amenazas complejas.

**WAF vs. DDoS Protection:**  
- WAF protege aplicaciones web contra ataques específicos (e.g., inyección SQL), mientras que DDoS Protection mitiga ataques volumétricos que buscan saturar recursos.

**Logs y Azure Monitor:**  
- La mayoría de los servicios integran logs con Azure Monitor Logs (Log Analytics), permitiendo análisis centralizado. La exportación a Azure Storage es común para retención a largo plazo.

**Costos:**  
- Servicios como Azure AD, PIM, Conditional Access e Identity Protection requieren licencias Premium (P1 o P2). Azure Sentinel, Defender, WAF, DDoS Protection y Firewall Manager tienen costos basados en ingesta, almacenamiento y uso de recursos.

---

## ✅ Mejores Prácticas

**Azure AD:**  
- Habilita retención extendida de logs para auditorías de cumplimiento.  
- Usa MFA y políticas de Conditional Access para todos los usuarios.  

**PIM:**  
- Configura aprobaciones manuales para activaciones de roles críticos.  
- Revisa periódicamente los logs de auditoría para detectar uso indebido.  

**Conditional Access:**  
- Define políticas basadas en riesgos y dispositivos para minimizar accesos no autorizados.  
- Monitorea logs de inicio de sesión para ajustar políticas.  

**Identity Protection:**  
- Configura respuestas automatizadas para riesgos altos (e.g., restablecimiento de contraseña).  
- Integra con Azure Sentinel para análisis avanzado de threats.  

**Threat Detection:**  
- Combina con Identity Protection para una detección más robusta.  
- Exporta logs a Azure Sentinel para correlación de eventos.  

**Azure Sentinel:**  
- Configura conectores para todas las fuentes de datos relevantes (Azure AD, Defender, firewalls).  
- Usa playbooks para automatizar respuestas a incidentes.  

**Azure Defender:**  
- Implementa todas las recomendaciones de seguridad antes de habilitar protección.  
- Integra logs con Sentinel para monitoreo continuo.  

**Infrastructure Protection:**  
- Revisa regularmente las recomendaciones de Microsoft Defender for Cloud.  
- Habilita monitoreo continuo para nuevos recursos.  

**WAF:**  
- Ajusta reglas para equilibrar protección y experiencia del usuario.  
- Monitorea logs para detectar patrones de ataques emergentes.  

**DDoS Protection:**  
- Habilita el plan Standard para aplicaciones críticas.  
- Configura alertas en Azure Monitor para ataques DDoS detectados.  

**Azure Firewall Manager:**  
- Centraliza políticas para simplificar la gestión de múltiples firewalls.  
- Exporta logs a Log Analytics para análisis detallado.  

**General:**  
- Usa Azure Monitor Logs para análisis centralizado de todos los servicios.  
- Configura alertas en Azure Monitor para eventos críticos.  
- Optimiza costos con políticas de retención en Azure Storage.  

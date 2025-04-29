# Análisis de Servicios de Logging en AWS

Este documento detalla los principales servicios de AWS relacionados con la generación y análisis de logs: **CloudTrail**, **CloudWatch**, **VPC Flow Logs**, **S3 Access Logs** y **Route 53 Logs**.

---

## 🛡️ AWS CloudTrail

**Propósito:**  
Registrar llamadas a la API y actividades de la cuenta para auditoría, cumplimiento y resolución de problemas.

**Tipos de Logs:**

- **Eventos de gestión:** Activados por defecto.
- **Eventos de datos:** Requieren activación.
- **Eventos de actividad de red:** Requieren configuración explícita.
- **Eventos Insights:** Detectan anomalías en la actividad.

**Almacenamiento:**

- Bucket S3.
- CloudWatch Logs.
- EventBridge.

**Casos de uso:**

- Investigar quién cambió permisos de un bucket S3.
- Rastrear intentos de acceso no autorizados a recursos.

**Costo:**

- Eventos de gestión: Gratuitos por 90 días.
- Eventos de datos y almacenamiento en S3: Tienen costo.

---

## 📊 Amazon CloudWatch

**Propósito:**  
Monitorear métricas, logs y eventos de recursos y aplicaciones AWS.

**Tipos de Logs:**

- CloudWatch Logs: Almacena logs de aplicaciones, servicios AWS o sistemas personalizados.
- CloudWatch Logs Insights: Permite consultas tipo SQL para analizar logs.
- Logs de métricas: Generan métricas personalizadas a partir de logs.
- VPC Flow Logs: Logs de tráfico de red que pueden enviarse a CloudWatch Logs.

**Almacenamiento:**

- Grupos de logs de CloudWatch con períodos de retención configurables de 1 día a 10 años.

**Casos de uso:**

- Crear alertas para uso elevado de CPU en instancias EC2.
- Analizar logs de aplicaciones para identificar errores.

**Costo:**

- Basado en ingesta, almacenamiento y consultas.
- Las métricas y alertas tienen costos adicionales.

---

## 🌐 VPC Flow Logs

**Propósito:**  
Capturar información sobre el tráfico IP que entra y sale de las interfaces de red en una VPC.

**Tipos de Logs:**

- Tráfico aceptado: Registra tráfico permitido por grupos de seguridad o ACLs de red.
- Tráfico rechazado: Registra tráfico bloqueado por grupos de seguridad o ACLs.
- Todo el tráfico: Incluye tanto tráfico aceptado como rechazado.

**Almacenamiento:**

- CloudWatch Logs.
- S3.
- Amazon Data Firehose.

**Casos de uso:**

- Detectar intentos de acceso no autorizados a una VPC.
- Diagnosticar reglas de grupos de seguridad demasiado restrictivas.

**Costo:**

- Basado en la cantidad de datos registrados y almacenados.

---

## 📁 S3 Access Logs

**Propósito:**  
Registrar detalles de las solicitudes realizadas a un bucket S3.

**Tipos de Logs:**

- Logs de solicitudes: Capturan detalles de cada solicitud, incluyendo solicitante, tipo de solicitud, nombre del bucket/objeto, estado HTTP, códigos de error y marca de tiempo.
- Logs a nivel de bucket: Registran acciones en el bucket (como PutBucketPolicy).
- Logs a nivel de objeto: Registran acciones en objetos (como GetObject), que se solapan con los eventos de datos de CloudTrail pero incluyen campos adicionales como bytes transferidos.

**Almacenamiento:**

- Entrega en un bucket S3 designado.
- La entrega es de "mejor esfuerzo" y puede tener retrasos.

**Casos de uso:**

- Rastrear acceso a objetos sensibles en un bucket S3.
- Analizar patrones de solicitudes para optimizar costos.

**Costo:**

- Basado en el almacenamiento en S3 y consultas en Athena (si se usa).

**Nota:**

- No habilites logs en el bucket de destino para evitar registros recursivos.

---

## 🧭 Amazon Route 53 Logs

**Propósito:**  
Registrar consultas DNS y llamadas API, ayudando a resolver problemas de DNS, monitorear actividad de dominios y garantizar seguridad y cumplimiento.

**Tipos de Logs:**

- Logs de consultas DNS: Capturan consultas DNS a dominios alojados en Route 53, incluyendo nombre de la consulta, tipo de consulta, IP de origen, código de respuesta y marca de tiempo.
- Logs de llamadas API de Route 53: Registran acciones API vía CloudTrail, con detalles como solicitante y dirección IP.
- Métricas de comprobaciones de salud: Métricas de comprobaciones de salud de Route 53, publicadas en CloudWatch para monitorear disponibilidad.

**Almacenamiento:**

- Logs de consultas DNS en CloudWatch Logs.
- Logs de API en CloudTrail (S3 o CloudWatch Logs).
- Métricas de salud en CloudWatch Metrics.

**Casos de uso:**

- Investigar patrones de consultas DNS para un dominio.
- Auditar quién creó una nueva zona alojada.

**Costo:**

- Basado en la ingesta de logs en CloudWatch y almacenamiento en S3 (para CloudTrail).

---

## 📋 Tabla Resumen

| Servicio       | Propósito                                 | Tipos de Logs                                | Almacenamiento                         | Ejemplo de Uso                             |
|----------------|-------------------------------------------|----------------------------------------------|----------------------------------------|--------------------------------------------|
| CloudTrail     | Auditar llamadas API y actividad          | Gestión, Datos, Actividad de Red, Insights   | S3, CloudWatch Logs, EventBridge       | Rastrear eliminación de bucket S3          |
| CloudWatch     | Monitorear métricas, logs y eventos       | Logs de aplicaciones, Métricas, VPC Flow Logs| Grupos de logs CloudWatch              | Alertar sobre alto uso de CPU en EC2       |
| VPC Flow Logs  | Monitorear tráfico de red en VPC          | Aceptado, Rechazado, Todo                    | CloudWatch Logs, S3, Data Firehose     | Detectar accesos no autorizados a VPC      |
| S3 Access Logs | Registrar solicitudes a buckets S3        | Solicitudes (bucket/objeto)                  | Bucket S3                              | Auditar acceso a objetos sensibles         |
| Route 53       | Registrar consultas DNS y API             | Consultas DNS, Llamadas API, Métricas de Salud| CloudWatch Logs, CloudTrail (S3)       | Resolver fallos en consultas DNS           |

---

## 📝 Notas Clave

**CloudTrail vs. CloudWatch:**

- CloudTrail audita actividades API (por ejemplo, quién hizo un bucket público), mientras que CloudWatch monitorea rendimiento y agrega logs (por ejemplo, alerta sobre uso elevado de un bucket).

**S3 Access Logs vs. CloudTrail:**

- S3 Access Logs son más detallados, pero su entrega es menos confiable e incluyen bytes transferidos. CloudTrail (eventos de datos) es ideal para auditoría de seguridad, pero requiere activación.

**Consultas Estructuradas:**

- CloudWatch Logs Insights ofrece sintaxis similar a SQL para logs en CloudWatch, mientras que Amazon Athena utiliza SQL estándar para logs en S3 (CloudTrail, S3 Access Logs).

**Costos:**

- CloudTrail ofrece eventos de gestión gratuitos por 90 días; eventos de datos y S3 tienen costo. CloudWatch tiene costos por ingesta, almacenamiento y consultas. VPC Flow Logs y S3 Access Logs tienen costos por datos registrados y almacenados. Route 53 tiene costos por logs DNS en CloudWatch y almacenamiento en CloudTrail.

---

## ✅ Mejores Prácticas

**CloudTrail:**

- Configura trails multi-región para auditoría completa.
- Habilita eventos de datos solo para recursos críticos.

**CloudWatch:**

- Define períodos de retención cortos para logs no críticos.
- Utiliza métricas personalizadas para reducir consultas.

**VPC Flow Logs:**

- Personaliza campos para minimizar datos registrados.
- Analiza tráfico rechazado para ajustar reglas de seguridad.

**S3 Access Logs:**

- Utiliza buckets separados para logs.
- Habilita políticas de ciclo de vida.
- Combina con CloudTrail para auditorías completas.

**Route 53:**

- Monitorea consultas DNS para detectar typosquatting o ataques.
- Utiliza métricas de salud para garantizar disponibilidad.

**General:**

- Emplea Amazon Athena o CloudWatch Logs Insights para análisis eficiente.
- Configura alertas en CloudWatch para eventos críticos.
- Optimiza costos con políticas de ciclo de vida en S3.

# MultiCloud

# ☁️ Cloud Logging & Metrics: AWS, Azure, GCP

## 📖 ¿Qué encontrarás en este repositorio?

Este repositorio es una guía práctica para trabajar con **logging y métricas** en **AWS**, **Azure**, y **Google Cloud Platform (GCP)**, con un enfoque en monitoreo, auditoría y **resolución de incidentes**. Contiene documentación detallada de los servicios clave de logging y métricas, incluyendo tipos de logs, métricas, almacenamiento, casos de uso y pasos para investigar incidentes. También incluye reglas Sigma detalladas en [`sigma.md`](./sigma.md) para detectar amenazas en entornos de nube, ideales para equipos de seguridad, DevOps y administradores.

## 📂 Estructura del Repositorio

- [`aws.md`](./aws.md): Guía de logging y métricas en AWS.
- [`azure.md`](./azure.md): Guía de logging y métricas en Azure.
- [`gcp.md`](./gcp.md): Guía de logging y métricas en GCP.
- [`incident-aws.md`](./incident-aws.md): Investigación de incidentes de seguridad en AWS.
- [`incident-azure.md`](./incident-azure.md): Investigación de incidentes de seguridad en Azure.
- [`incident-gcp.md`](./incident-gcp.md): Investigación de incidentes de seguridad en GCP.
- [`incident-aws-principals.md`](./incident-aws-principals.md): Detalles de entidades (principals) involucradas en incidentes de AWS.
- [`incident-azure-principals.md`](./incident-azure-principals.md): Detalles de entidades (principals) involucradas en incidentes de Azure.
- [`incident-gcp-principals.md`](./incident-gcp-principals.md): Detalles de entidades (principals) involucradas en incidentes de GCP.
- [`logs.md`](./logs.md): Información general sobre logging y métricas en la nube.
- [`sigma.md`](./sigma.md): Reglas Sigma para detección de amenazas en AWS, Azure, y GCP.

## 📋 Principales Incidentes

### AWS
- 📁 [Pérdida Crítica de Datos en S3: Evento de Eliminación Inesperada de Bucket](./incident-aws.md#perdida-critica-de-datos-en-s3)
- 🔑 [Aprovisionamiento Sospechoso de Perfil de Instancia IAM](./incident-aws.md#aprovisionamiento-sospechoso-de-perfil-de-instancia-iam)
- 🔒 [Modificación No Autorizada de Políticas IAM Detectada](./incident-aws.md#modificacion-no-autorizada-de-politicas-iam)
- 🛠️ [Ejecución Sospechosa de Función Lambda Identificada](./incident-aws.md#ejecucion-sospechosa-de-funcion-lambda)
- 🤝 [Relación de Confianza Cruzada Sospechosa Identificada](./incident-aws.md#relacion-de-confianza-cruzada-sospechosa)

### Azure
- 🔍 [Actividad Inusual de Solicitud LISTKEYS Registrada](./incident-azure.md#actividad-inusual-de-solicitud-listkeys)
- 🔐 [Configuración No Aprobada de Grupo de Seguridad de Red Detectada](./incident-azure.md#configuracion-no-aprobada-de-grupo-de-seguridad-de-red)
- 📦 [Enumeración de Recursos de Contenedores de Azure](./incident-azure.md#enumeracion-de-recursos-de-contenedores-de-azure)
- 🔐 [Posible Exfiltración de Secretos desde Azure Key Vault Identificada](./incident-azure.md#posible-exfiltracion-de-secretos-desde-azure-key-vault)
- 🔑 [Actualización Sospechosa de Credenciales de Registro de Aplicaciones Detectada](./incident-azure.md#actualizacion-sospechosa-de-credenciales-de-registro-de-aplicaciones)

### GCP
- 📁 [Posible Exfiltración de Datos: Copia No Aprobada de Archivo desde un Bucket de GCP](./incident-gcp.md#posible-exfiltracion-de-datos-copia-no-aprobada-de-archivo-desde-un-bucket-de-gcp)
- 🔑 [Suplantación Sospechosa de Cuenta de Servicio](./incident-gcp.md#suplantacion-sospechosa-de-cuenta-de-servicio)
- 🔒 [Asignación Sospechosa de Rol IAM Detectada](./incident-gcp.md#asignacion-sospechosa-de-rol-iam)
- 📤 [Actividad Anormal de Creación de Objetos en Bucket Identificada](./incident-gcp.md#actividad-anormal-de-creacion-de-objetos-en-bucket)
- 🔑 [Actividad Sospechosa de Creación de Secretos de Cuenta de Servicio Identificada](./incident-gcp.md#actividad-sospechosa-de-creacion-de-secretos-de-cuenta-de-servicio)

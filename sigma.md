## 🔍 Generación de Reglas Sigma para Incidentes de Seguridad en AWS, Azure y GCP

### 📁 Pérdida Crítica de Datos en S3: Evento de Eliminación Inesperada de Bucket
- **AWS CloudTrail**  
  - **Acción de Evento**: `DeleteBucket`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar eliminaciones no autorizadas de buckets S3 que causen pérdida de datos críticos.  
  - **Regla Sigma**:  
    - **Campos Clave**: `eventSource="s3.amazonaws.com"`, `eventName="DeleteBucket"`, `userIdentity.arn`, `sourceIPAddress`, `requestParameters.bucketName`.  
    - **Condiciones de Detección**: Coincidencia exacta de `eventName="DeleteBucket"`. Excluye usuarios o roles IAM autorizados (lista blanca). Alerta si `sourceIPAddress` no está en rangos conocidos o si el evento ocurre fuera de horarios laborales.  
    - **Severidad**: Crítica (pérdida de datos irreversible).  
    - **Query Sigma Genérica**: Filtra eventos donde `eventSource` sea `s3.amazonaws.com` y `eventName` sea `DeleteBucket`. Añade condiciones para `userIdentity.arn` no en lista blanca de administradores y `sourceIPAddress` fuera de rangos corporativos. Configura un umbral para un solo evento y dispara una alerta.

### 🔑 Aprovisionamiento Sospechoso de Perfil de Instancia IAM
- **AWS CloudTrail**  
  - **Acción de Evento**: `CreateInstanceProfile`  
  - **Evento**: Management Event  
  - **Relevancia**: Investigar creación de perfiles de instancia IAM que otorguen permisos indebidos a instancias EC2.  
  - **Regla Sigma**:  
    - **Campos Clave**: `eventSource="iam.amazonaws.com"`, `eventName="CreateInstanceProfile"`, `userIdentity.arn`, `requestParameters.instanceProfileName`, `sourceIPAddress`.  
    - **Condiciones de Detección**: Coincidencia de `eventName="CreateInstanceProfile"`. Excluye usuarios autorizados. Alerta si el perfil otorga permisos elevados (e.g., `AdministratorAccess`) o si `sourceIPAddress` es inusual.  
    - **Severidad**: Alta (posible escalación de privilegios).  
    - **Query Sigma Genérica**: Filtra eventos con `eventSource="iam.amazonaws.com"` y `eventName="CreateInstanceProfile"`. Añade condiciones para `userIdentity.arn` no autorizado y verifica `requestParameters.instanceProfileName` para permisos sospechosos. Dispara una alerta para un solo evento desde IPs desconocidas.

### 🔒 Modificación No Autorizada de Políticas IAM Detectada
- **AWS CloudTrail**  
  - **Acción de Evento**: `UpdateRolePolicy`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar cambios en permisos de roles IAM que permitan accesos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `eventSource="iam.amazonaws.com"`, `eventName="UpdateRolePolicy"`, `userIdentity.arn`, `requestParameters.roleName`, `requestParameters.policyDocument`.  
    - **Condiciones de Detección**: Coincidencia de `eventName="UpdateRolePolicy"`. Alerta si `requestParameters.policyDocument` incluye permisos elevados (e.g., `*`) o si `userIdentity.arn` no está autorizado. Correlaciona con `sourceIPAddress` inusual.  
    - **Severidad**: Alta (riesgo de escalación de privilegios).  
    - **Query Sigma Genérica**: Filtra eventos con `eventSource="iam.amazonaws.com"` y `eventName="UpdateRolePolicy"`. Verifica que `requestParameters.policyDocument` contenga permisos amplios y que `userIdentity.arn` no esté en lista blanca. Dispara una alerta para eventos individuales.

### 🛠️ Ejecución Sospechosa de Función Lambda Identificada
- **AWS CloudTrail**  
  - **Acción de Evento**: `Invoke`  
  - **Evento**: Data Event  
  - **Relevancia**: Investigar invocaciones no autorizadas de funciones Lambda que ejecuten código malicioso.  
  - **Regla Sigma**:  
    - **Campos Clave**: `eventSource="lambda.amazonaws.com"`, `eventName="Invoke"`, `userIdentity.arn`, `requestParameters.functionName`, `sourceIPAddress`.  
    - **Condiciones de Detección**: Coincidencia de `eventName="Invoke"`. Alerta si `userIdentity.arn` no está autorizado, `sourceIPAddress` es inusual, o si la función tiene permisos elevados. Correlaciona con picos de invocaciones.  
    - **Severidad**: Media (depende del código ejecutado).  
    - **Query Sigma Genérica**: Filtra eventos con `eventSource="lambda.amazonaws.com"` y `eventName="Invoke"`. Añade condiciones para `userIdentity.arn` no autorizado y `sourceIPAddress` fuera de rangos conocidos. Configura un umbral para múltiples invocaciones en un corto período.

### 🤝 Relación de Confianza Cruzada Sospechosa Identificada
- **AWS CloudTrail**  
  - **Acción de Evento**: `CreateRole`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar creación de roles IAM con relaciones de confianza cruzadas que permitan accesos externos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `eventSource="iam.amazonaws.com"`, `eventName="CreateRole"`, `userIdentity.arn`, `requestParameters.trustPolicy`, `userIdentity.accountId`.  
    - **Condiciones de Detección**: Coincidencia de `eventName="CreateRole"`. Alerta si `requestParameters.trustPolicy` incluye cuentas externas (no en `userIdentity.accountId`) o si `userIdentity.arn` no está autorizado.  
    - **Severidad**: Alta (riesgo de acceso externo).  
    - **Query Sigma Genérica**: Filtra eventos con `eventSource="iam.amazonaws.com"` y `eventName="CreateRole"`. Verifica que `requestParameters.trustPolicy` contenga cuentas externas y que `userIdentity.arn` no esté en lista blanca. Dispara una alerta para un solo evento.

### 🔍 Actividad Inusual de Solicitud LISTKEYS Registrada
- **Azure Audit Logs**  
  - **Acción de Evento**: `Microsoft.Storage/storageAccounts/listKeys/action`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar solicitudes para listar claves de almacenamiento que indiquen intentos de acceso no autorizado.  
  - **Regla Sigma**:  
    - **Campos Clave**: `Category="Administrative"`, `OperationName="List Storage Account Keys"`, `CallerIpAddress`, `Caller`, `ResourceId`.  
    - **Condiciones de Detección**: Coincidencia de `OperationName="List Storage Account Keys"`. Alerta si `CallerIpAddress` está fuera de rangos conocidos o si `Caller` no está en lista blanca de administradores. Correlaciona con múltiples solicitudes en un corto período.  
    - **Severidad**: Alta (riesgo de acceso a datos sensibles).  
    - **Query Sigma Genérica**: Filtra eventos con `Category="Administrative"` y `OperationName="List Storage Account Keys"`. Añade condiciones para `CallerIpAddress` no en rangos corporativos y `Caller` no autorizado. Configura un umbral para múltiples solicitudes en minutos.

### 🔐 Configuración No Aprobada de Grupo de Seguridad de Red Detectada
- **Azure Audit Logs**  
  - **Acción de Evento**: `Microsoft.Network/networkSecurityGroups/write`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar cambios en reglas de NSG que permitan accesos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `Category="Administrative"`, `OperationName="Create or Update Network Security Group"`, `Caller`, `ResourceId`, `Properties.SecurityRules`.  
    - **Condiciones de Detección**: Coincidencia de `OperationName="Create or Update Network Security Group"`. Alerta si `Properties.SecurityRules` permite tráfico entrante amplio (e.g., puerto 3389 desde `0.0.0.0/0`) o si `Caller` no está autorizado.  
    - **Severidad**: Alta (exposición de red).  
    - **Query Sigma Genérica**: Filtra eventos con `Category="Administrative"` y `OperationName="Create or Update Network Security Group"`. Verifica que `Properties.SecurityRules` contenga reglas permisivas y que `Caller` no esté en lista blanca. Dispara una alerta para un solo evento.

### 📦 Enumeración de Recursos de Contenedores de Azure
- **Azure Audit Logs**  
  - **Acción de Evento**: `Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar solicitudes para listar credenciales de clústeres AKS que indiquen intentos de enumeración no autorizada.  
  - **Regla Sigma**:  
    - **Campos Clave**: `Category="Administrative"`, `OperationName="List Cluster Admin Credential"`, `CallerIpAddress`, `Caller`, `ResourceId`.  
    - **Condiciones de Detección**: Coincidencia de `OperationName="List Cluster Admin Credential"`. Alerta si `CallerIpAddress` es inusual o si `Caller` no está autorizado. Correlaciona con múltiples solicitudes en un corto período.  
    - **Severidad**: Media (riesgo de enumeración de recursos).  
    - **Query Sigma Genérica**: Filtra eventos con `Category="Administrative"` y `OperationName="List Cluster Admin Credential"`. Añade condiciones para `CallerIpAddress` fuera de rangos conocidos y `Caller` no autorizado. Configura un umbral para múltiples solicitudes.

### 🔐 Posible Exfiltración de Secretos desde Azure Key Vault Identificada
- **Azure Audit Logs**  
  - **Acción de Evento**: `Microsoft.KeyVault/vaults/secrets/read`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a secretos en Key Vault que indiquen exfiltración.  
  - **Regla Sigma**:  
    - **Campos Clave**: `Category="Audit"`, `OperationName="SecretGet"`, `CallerIpAddress`, `Caller`, `Properties.SecretName`.  
    - **Condiciones de Detección**: Coincidencia de `OperationName="SecretGet"`. Alerta si `CallerIpAddress` está fuera de rangos conocidos, `Caller` no está autorizado, o si hay múltiples accesos a secretos críticos en un corto período.  
    - **Severidad**: Crítica (exfiltración de secretos).  
    - **Query Sigma Genérica**: Filtra eventos con `Category="Audit"` y `OperationName="SecretGet"`. Verifica que `CallerIpAddress` no esté en rangos corporativos y `Caller` no autorizado. Configura un umbral para múltiples accesos a `Properties.SecretName`.

### 🔑 Actualización Sospechosa de Credenciales de Registro de Aplicaciones Detectada
- **Azure Audit Logs**  
  - **Acción de Evento**: `Microsoft.AAD/applications/credentials/update`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar cambios en credenciales de aplicaciones que permitan accesos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `Category="Administrative"`, `OperationName="Update application - Certificates and secrets management"`, `Caller`, `Properties.AppId`.  
    - **Condiciones de Detección**: Coincidencia de `OperationName="Update application - Certificates and secrets management"`. Alerta si `Caller` no está autorizado o si `CallerIpAddress` es inusual. Correlaciona con accesos posteriores a recursos protegidos.  
    - **Severidad**: Alta (riesgo de compromiso de aplicaciones).  
    - **Query Sigma Genérica**: Filtra eventos con `Category="Administrative"` y `OperationName="Update application - Certificates and secrets management"`. Añade condiciones para `Caller` no autorizado y `CallerIpAddress` fuera de rangos. Dispara una alerta para un solo evento.

### 📁 Posible Exfiltración de Datos: Copia No Aprobada de Archivo desde un Bucket de GCP
- **GCP Cloud Audit Logs**  
  - **Acción de Evento**: `storage.objects.get`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a objetos en Cloud Storage que indiquen exfiltración de datos.  
  - **Regla Sigma**:  
    - **Campos Clave**: `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"`, `protoPayload.methodName="storage.objects.get"`, `protoPayload.authenticationInfo.principalEmail`, `resource.labels.bucket_name`, `protoPayload.requestMetadata.callerIp`.  
    - **Condiciones de Detección**: Coincidencia de `methodName="storage.objects.get"`. Alerta si `principalEmail` no está autorizado, `callerIp` está fuera de rangos conocidos, o si hay múltiples accesos a buckets sensibles en un corto período.  
    - **Severidad**: Crítica (exfiltración de datos).  
    - **Query Sigma Genérica**: Filtra eventos con `logName` conteniendo `data_access` y `methodName="storage.objects.get"`. Añade condiciones para `principalEmail` no autorizado y `callerIp` fuera de rangos corporativos. Configura un umbral para múltiples accesos a `bucket_name`.

### 🔑 Suplantación Sospechosa de Cuenta de Servicio
- **GCP Cloud Audit Logs**  
  - **Acción de Evento**: `iam.serviceAccounts.actAs`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar intentos de suplantación de cuentas de servicio para acceder a recursos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"`, `protoPayload.methodName="google.iam.credentials.v1.GenerateAccessToken"`, `protoPayload.authenticationInfo.principalEmail`, `protoPayload.requestMetadata.callerIp`.  
    - **Condiciones de Detección**: Coincidencia de `methodName="google.iam.credentials.v1.GenerateAccessToken"`. Alerta si `principalEmail` no está autorizado o si `callerIp` es inusual. Correlaciona con accesos posteriores a recursos críticos.  
    - **Severidad**: Alta (escalación de privilegios).  
    - **Query Sigma Genérica**: Filtra eventos con `logName` conteniendo `data_access` y `methodName="google.iam.credentials.v1.GenerateAccessToken"`. Verifica que `principalEmail` no esté en lista blanca y `callerIp` fuera de rangos. Dispara una alerta para un solo evento.

### 🔒 Asignación Sospechosa de Rol IAM Detectada
- **GCP Cloud Audit Logs**  
  - **Acción de Evento**: `iam.serviceAccounts.setIamPolicy`  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Investigar cambios en permisos de cuentas de servicio que permitan accesos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `logName:"/logs/cloudaudit.googleapis.com%2Factivity"`, `protoPayload.methodName="SetIamPolicy"`, `protoPayload.serviceName="iam.googleapis.com"`, `protoPayload.authenticationInfo.principalEmail`, `protoPayload.authorizationInfo`.  
    - **Condiciones de Detección**: Coincidencia de `methodName="SetIamPolicy"` y `serviceName="iam.googleapis.com"`. Alerta si `authorizationInfo` otorga permisos elevados (e.g., `roles/owner`) o si `principalEmail` no está autorizado.  
    - **Severidad**: Alta (riesgo de escalación de privilegios).  
    - **Query Sigma Genérica**: Filtra eventos con `logName` conteniendo `activity` y `methodName="SetIamPolicy" serviceName="iam.googleapis.com"`. Verifica que `authorizationInfo` contenga permisos amplios y que `principalEmail` no esté en lista blanca. Dispara una alerta para un solo evento.

### 📤 Actividad Anormal de Creación de Objetos en Bucket Identificada
- **GCP Cloud Audit Logs**  
  - **Acción de Evento**: `storage.objects.create`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar cargas sospechosas o subidas de malware a buckets de Cloud Storage.  
  - **Regla Sigma**:  
    - **Campos Clave**: `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"`, `protoPayload.methodName="storage.objects.create"`, `protoPayload.authenticationInfo.principalEmail`, `resource.labels.bucket_name`, `protoPayload.requestMetadata.callerIp`.  
    - **Condiciones de Detección**: Coincidencia de `methodName="storage.objects.create"`. Alerta si `principalEmail` no está autorizado, `callerIp` está fuera de rangos conocidos, o si hay múltiples creaciones en un corto período.  
    - **Severidad**: Media (depende del contenido subido).  
    - **Query Sigma Genérica**: Filtra eventos con `logName` conteniendo `data_access` y `methodName="storage.objects.create"`. Añade condiciones para `principalEmail` no autorizado y `callerIp` fuera de rangos. Configura un umbral para múltiples creaciones en minutos.

### 🔑 Actividad Sospechosa de Creación de Secretos de Cuenta de Servicio Identificada
- **GCP Cloud Audit Logs**  
  - **Acción de Evento**: `iam.serviceAccounts.keys.create`  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar creación de claves de cuentas de servicio que puedan usarse para accesos no autorizados.  
  - **Regla Sigma**:  
    - **Campos Clave**: `logName:"/logs/cloudaudit.googleapis.com%2Factivity"`, `protoPayload.methodName="google.iam.admin.v1.CreateServiceAccountKey"`, `protoPayload.authenticationInfo.principalEmail`, `protoPayload.requestMetadata.callerIp`.  
    - **Condiciones de Detección**: Coincidencia de `methodName="google.iam.admin.v1.CreateServiceAccountKey"`. Alerta si `principalEmail` no está autorizado o si `callerIp` es inusual. Correlaciona con accesos posteriores usando la clave.  
    - **Severidad**: Alta (riesgo de compromiso de cuenta de servicio).  
    - **Query Sigma Genérica**: Filtra eventos con `logName` conteniendo `activity` y `methodName="google.iam.admin.v1.CreateServiceAccountKey"`. Verifica que `principalEmail` no esté en lista blanca y `callerIp` fuera de rangos. Dispara una alerta para un solo evento.

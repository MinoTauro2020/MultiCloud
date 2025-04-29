## 🔍 Lista de Incidentes de Seguridad con Reglas Sigma Técnicas en AWS, Azure y GCP

### AWS
- **📁 Pérdida Crítica de Datos en S3: Evento de Eliminación Inesperada de Bucket**  
  - **Acción de Evento**: `DeleteBucket`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar eliminaciones no autorizadas de buckets S3 que causen pérdida de datos críticos.  
  - **Query Sigma Específica**: Filtra eventos con `eventSource="s3.amazonaws.com" AND eventName="DeleteBucket"`. Excluye `userIdentity.arn IN ("arn:aws:iam::[account_id]:role/admin-role", "arn:aws:iam::[account_id]:user/approved-user")`. Añade condición `sourceIPAddress !~ "192\.168\.\d{1,3}\.\d{1,3}|10\.\d{1,3}\.\d{1,3}\.\d{1,3}"` para IPs no corporativas. Correlaciona con ausencia de `eventName="PutBucketPolicy"` en ventana de 1 hora previa (`NOT EXISTS (eventSource="s3.amazonaws.com" AND eventName="PutBucketPolicy")`). Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **🔑 Aprovisionamiento Sospechoso de Perfil de Instancia IAM**  
  - **Acción de Evento**: `CreateInstanceProfile`  
  - **Evento**: Management Event  
  - **Relevancia**: Investigar creación de perfiles de instancia IAM que otorguen permisos indebidos a instancias EC2.  
  - **Query Sigma Específica**: Filtra eventos con `eventSource="iam.amazonaws.com" AND eventName="CreateInstanceProfile"`. Excluye `userIdentity.arn IN ("arn:aws:iam::[account_id]:role/approved-iam-admin")`. Añade condición `requestParameters.roleArn =~ ".*:role/.*(Admin|PowerUser|FullAccess).*"`. Verifica `sourceIPAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Correlaciona con `eventName="RunInstances" AND requestParameters.instanceProfileArn =~ requestParameters.instanceProfileName` en 30 minutos. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **🔒 Modificación No Autorizada de Políticas IAM Detectada**  
  - **Acción de Evento**: `UpdateRolePolicy`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar cambios en permisos de roles IAM que permitan accesos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `eventSource="iam.amazonaws.com" AND eventName="UpdateRolePolicy"`. Añade condición `requestParameters.policyDocument =~ ".*\"Effect\":\"Allow\".*\"Action\":\"\\*\".*\"Resource\":\"\\*\".*"`. Excluye `userIdentity.arn IN ("arn:aws:iam::[account_id]:role/approved-iam-admin")`. Verifica `sourceIPAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Correlaciona con `eventName="AssumeRole" AND requestParameters.roleArn =~ requestParameters.roleName` en 1 hora. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **🛠️ Ejecución Sospechosa de Función Lambda Identificada**  
  - **Acción de Evento**: `Invoke`  
  - **Evento**: Data Event  
  - **Relevancia**: Investigar invocaciones no autorizadas de funciones Lambda que ejecuten código malicioso.  
  - **Query Sigma Específica**: Filtra eventos con `eventSource="lambda.amazonaws.com" AND eventName="Invoke"`. Excluye `userIdentity.arn IN ("arn:aws:iam::[account_id]:role/approved-lambda-exec")`. Añade condición `sourceIPAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Configura umbral para `COUNT(requestParameters.functionName) >= 5` en 10 minutos. Correlaciona con `eventName="CreateFunction" AND requestParameters.functionName =~ requestParameters.functionName` en 24 horas. Dispara alerta si se cumple.

- **🤝 Relación de Confianza Cruzada Sospechosa Identificada**  
  - **Acción de Evento**: `CreateRole`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar creación de roles IAM con relaciones de confianza cruzadas que permitan accesos externos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `eventSource="iam.amazonaws.com" AND eventName="CreateRole"`. Añade condición `requestParameters.trustPolicy =~ ".*\"Principal\":\\{\"AWS\":\"arn:aws:iam::[0-9]{12}:[^\"]*\"\\}.*"` AND `userIdentity.accountId != requestParameters.trustPolicy.accountId`. Excluye `userIdentity.arn IN ("arn:aws:iam::[account_id]:role/approved-iam-admin")`. Verifica `sourceIPAddress !~ "^192\.168\.0\.0/16"`. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

### Azure
- **🔍 Actividad Inusual de Solicitud LISTKEYS Registrada**  
  - **Acción de Evento**: `Microsoft.Storage/storageAccounts/listKeys/action`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar solicitudes para listar claves de almacenamiento que indiquen intentos de acceso no autorizado.  
  - **Query Sigma Específica**: Filtra eventos con `Category="Administrative" AND OperationName="List Storage Account Keys"`. Añade condición `NOT Caller IN ("approved-user@domain.com", "approved-service-principal")` AND `CallerIpAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Configura umbral para `COUNT(ResourceId) >= 3` en 15 minutos. Correlaciona con `OperationName="Get Blob"` en 1 hora para detectar uso de claves. Dispara alerta si se cumple.

- **🔐 Configuración No Aprobada de Grupo de Seguridad de Red Detectada**  
  - **Acción de Evento**: `Microsoft.Network/networkSecurityGroups/write`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar cambios en reglas de NSG que permitan accesos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `Category="Administrative" AND OperationName="Create or Update Network Security Group"`. Añade condición `Properties.SecurityRules =~ ".*\"access\":\"Allow\".*\"sourceAddressPrefix\":\"0\.0\.0\.0/0\".*\"destinationPortRange\":\"(22|3389|80|443).*"` AND `NOT Caller IN ("approved-admin@domain.com")`. Verifica `CallerIpAddress !~ "^10\.0\.0\.0/8"`. Correlaciona con `OperationName="Create or Update Network Security Group Rule"` en 1 hora. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **📦 Enumeración de Recursos de Contenedores de Azure**  
  - **Acción de Evento**: `Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar solicitudes para listar credenciales de clústeres AKS que indiquen intentos de enumeración no autorizada.  
  - **Query Sigma Específica**: Filtra eventos con `Category="Administrative" AND OperationName="List Cluster Admin Credential"`. Añade condición `NOT Caller IN ("approved-user@domain.com", "approved-service-principal")` AND `CallerIpAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Configura umbral para `COUNT(ResourceId) >= 2` en 10 minutos. Correlaciona con `OperationName="List Cluster User Credential"` en 30 minutos. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **🔐 Posible Exfiltración de Secretos desde Azure Key Vault Identificada**  
  - **Acción de Evento**: `Microsoft.KeyVault/vaults/secrets/read`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a secretos en Key Vault que indiquen exfiltración.  
  - **Query Sigma Específica**: Filtra eventos con `Category="Audit" AND OperationName="SecretGet"`. Añade condición `NOT Caller IN ("approved-service-principal", "approved-user@domain.com")` AND `CallerIpAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Configura umbral para `COUNT(Properties.SecretName) >= 5` en 15 minutos. Correlaciona con `OperationName="SecretSet"` en 1 hora para detectar modificaciones previas. Dispara alerta si se cumple.

- **🔑 Actualización Sospechosa de Credenciales de Registro de Aplicaciones Detectada**  
  - **Acción de Evento**: `Microsoft.AAD/applications/credentials/update`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar cambios en credenciales de aplicaciones que permitan accesos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `Category="Administrative" AND OperationName="Update application - Certificates and secrets management"`. Añade condición `NOT Caller IN ("approved-admin@domain.com")` AND `CallerIpAddress !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Correlaciona con `Category="SignInLogs" AND ResultType="0" AND Properties.AppId =~ Properties.AppId` en 1 hora para detectar uso de credenciales. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

### GCP
- **📁 Posible Exfiltración de Datos: Copia No Aprobada de Archivo desde un Bucket de GCP**  
  - **Acción de Evento**: `storage.objects.get`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a objetos en Cloud Storage que indiquen exfiltración de datos.  
  - **Query Sigma Específica**: Filtra eventos con `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access" AND protoPayload.methodName="storage.objects.get"`. Añade condición `NOT protoPayload.authenticationInfo.principalEmail IN ("approved-service-account@project.iam.gserviceaccount.com", "approved-user@domain.com")` AND `protoPayload.requestMetadata.callerIp !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Configura umbral para `COUNT(resource.labels.bucket_name) >= 5` en 10 minutos. Correlaciona con `methodName="storage.objects.list"` en 1 hora. Dispara alerta si se cumple.

- **🔑 Suplantación Sospechosa de Cuenta de Servicio**  
  - **Acción de Evento**: `iam.serviceAccounts.actAs`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar intentos de suplantación de cuentas de servicio para acceder a recursos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access" AND protoPayload.methodName="google.iam.credentials.v1.GenerateAccessToken"`. Añade condición `NOT protoPayload.authenticationInfo.principalEmail IN ("approved-service-account@project.iam.gserviceaccount.com")` AND `protoPayload.requestMetadata.callerIp !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Correlaciona con `methodName="storage.objects.get" OR methodName="compute.instances.insert"` en 1 hora. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **🔒 Asignación Sospechosa de Rol IAM Detectada**  
  - **Acción de Evento**: `iam.serviceAccounts.setIamPolicy`  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Investigar cambios en permisos de cuentas de servicio que permitan accesos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `logName:"/logs/cloudaudit.googleapis.com%2Factivity" AND protoPayload.methodName="SetIamPolicy" AND protoPayload.serviceName="iam.googleapis.com"`. Añade condición `protoPayload.authorizationInfo =~ ".*roles/(owner|editor|admin).*" AND NOT protoPayload.authenticationInfo.principalEmail IN ("approved-admin@domain.com")`. Verifica `protoPayload.requestMetadata.callerIp !~ "^10\.0\.0\.0/8"`. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

- **📤 Actividad Anormal de Creación de Objetos en Bucket Identificada**  
  - **Acción de Evento**: `storage.objects.create`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar cargas sospechosas o subidas de malware a buckets de Cloud Storage.  
  - **Query Sigma Específica**: Filtra eventos con `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access" AND protoPayload.methodName="storage.objects.create"`. Añade condición `NOT protoPayload.authenticationInfo.principalEmail IN ("approved-service-account@project.iam.gserviceaccount.com")` AND `protoPayload.requestMetadata.callerIp !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Configura umbral para `COUNT(resource.labels.bucket_name) >= 10` en 15 minutos. Correlaciona con `methodName="storage.objects.get"` en 1 hora. Dispara alerta si se cumple.

- **🔑 Actividad Sospechosa de Creación de Secretos de Cuenta de Servicio Identificada**  
  - **Acción de Evento**: `iam.serviceAccounts.keys.create`  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar creación de claves de cuentas de servicio que puedan usarse para accesos no autorizados.  
  - **Query Sigma Específica**: Filtra eventos con `logName:"/logs/cloudaudit.googleapis.com%2Factivity" AND protoPayload.methodName="google.iam.admin.v1.CreateServiceAccountKey"`. Añade condición `NOT protoPayload.authenticationInfo.principalEmail IN ("approved-admin@domain.com", "approved-service-account@project.iam.gserviceaccount.com")` AND `protoPayload.requestMetadata.callerIp !~ "^10\.0\.0\.0/8|^192\.168\.0\.0/16"`. Correlaciona con `methodName="google.iam.credentials.v1.GenerateAccessToken"` en 1 hora. Umbral: 1 evento en 5 minutos. Dispara alerta si se cumple.

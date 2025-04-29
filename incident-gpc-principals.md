## 🔍 Investigación de Incidentes de Seguridad en GCP Cloud Audit Logs

- **📁 Posible Exfiltración de Datos: Copia No Aprobada de Archivo desde un Bucket de GCP**  
  - **Acción de Evento**: `storage.objects.get`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a objetos en Cloud Storage que indiquen exfiltración de datos.  
  - **Query**:  
    - **Cloud Logging**: En Log Explorer, filtra por `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="storage.objects.get"`. Agrega filtros para `resource.labels.bucket_name` o `protoPayload.authenticationInfo.principalEmail`.  
    - **Chronicle**: Usa búsqueda UDM, filtrando por `event.metadata.event_type="STORAGE_OBJECT_GET"` y `event.target.resource.resource_type="GCS_BUCKET"`.  
    - **Splunk**: Configura un filtro para `sourcetype="google:gcp:audit" methodName="storage.objects.get"`, con campos como `principalEmail`.  
    - **Elastic**: En Kibana, filtra por `gcp.audit.methodName:"storage.objects.get"`, con condiciones para `gcp.audit.authenticationInfo.principalEmail`.  
    - **Cloud Monitoring**: Crea una métrica basada en logs con `methodName="storage.objects.get"` para contar accesos y configurar alertas.

- **🔑 Suplantación Sospechosa de Cuenta de Servicio**  
  - **Acción de Evento**: `iam.serviceAccounts.actAs`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar intentos de suplantación de cuentas de servicio para acceder a recursos no autorizados.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="google.iam.credentials.v1.GenerateAccessToken"`. Revisa `protoPayload.authenticationInfo.principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="IAM_ACT_AS"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="google.iam.credentials.v1.GenerateAccessToken"`, con `principalEmail`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"google.iam.credentials.v1.GenerateAccessToken"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="google.iam.credentials.v1.GenerateAccessToken"` y alerta.

- **🔒 Asignación Sospechosa de Rol IAM Detectada**  
  - **Acción de Evento**: `iam.serviceAccounts.setIamPolicy`  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Investigar cambios en permisos de cuentas de servicio que permitan accesos no autorizados.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="SetIamPolicy" serviceName="iam.googleapis.com"`. Revisa `protoPayload.authorizationInfo`.  
    - **Chronicle**: Busca `event.metadata.event_type="IAM_SET_POLICY" event.target.resource.resource_type="IAM_SERVICE_ACCOUNT"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="SetIamPolicy" serviceName="iam.googleapis.com"`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"SetIamPolicy" gcp.audit.serviceName:"iam.googleapis.com"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="SetIamPolicy" serviceName="iam.googleapis.com"` y alerta.

- **📤 Actividad Anormal de Creación de Objetos en Bucket Identificada**  
  - **Acción de Evento**: `storage.objects.create`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar cargas sospechosas o subidas de malware a buckets de Cloud Storage.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="storage.objects.create"`. Incluye `resource.labels.bucket_name`.  
    - **Chronicle**: Busca `event.metadata.event_type="STORAGE_OBJECT_CREATE" event.target.resource.resource_type="GCS_BUCKET"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="storage.objects.create"`, filtrando por `resource.labels.bucket_name`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"storage.objects.create"` en Kibana, con `gcp.audit.resource.labels.bucket_name`.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="storage.objects.create"` y alerta sobre picos.

- **🔑 Actividad Sospechosa de Creación de Secretos de Cuenta de Servicio Identificada**  
  - **Acción de Evento**: `iam.serviceAccounts.keys.create`  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar creación de claves de cuentas de servicio que puedan usarse para accesos no autorizados.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="google.iam.admin.v1.CreateServiceAccountKey"`. Revisa `protoPayload.authenticationInfo.principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="IAM_SERVICE_ACCOUNT_KEY_CREATE"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="google.iam.admin.v1.CreateServiceAccountKey"`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"google.iam.admin.v1.CreateServiceAccountKey"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="google.iam.admin.v1.CreateServiceAccountKey"` y configura alertas.

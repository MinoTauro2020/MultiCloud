- **🗂️ `storage.objects.get`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados o extracción de datos sensibles en Cloud Storage.  
  - **Query**:  
    - **Cloud Logging**: En Log Explorer, filtra por `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="storage.objects.get"`. Agrega filtros por `resource.labels.bucket_name` o `protoPayload.authenticationInfo.principalEmail` para usuario o bucket específico.  
    - **Chronicle**: Usa la búsqueda UDM, filtrando por `event.metadata.event_type="STORAGE_OBJECT_GET"` y `event.target.resource.resource_type="GCS_BUCKET"`.  
    - **Splunk**: Configura un filtro para `sourcetype="google:gcp:audit"` y busca `methodName="storage.objects.get"` con campos como `principalEmail` para correlacionar usuarios.  
    - **Elastic**: En Kibana, filtra por `gcp.audit.methodName:"storage.objects.get"` y agrega condiciones para `gcp.audit.authenticationInfo.principalEmail`.  
    - **Cloud Monitoring**: Crea una métrica basada en logs con `methodName="storage.objects.get"` para contar accesos y configurar alertas.

- **📤 `storage.objects.create`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar cargas sospechosas o posibles subidas de malware a Cloud Storage.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="storage.objects.create"`. Incluye `resource.labels.bucket_name` para el bucket afectado.  
    - **Chronicle**: Busca `event.metadata.event_type="STORAGE_OBJECT_CREATE"` con `event.target.resource.resource_type="GCS_BUCKET"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="storage.objects.create"` y filtra por `resource.labels.bucket_name`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"storage.objects.create"` en Kibana, con condiciones para `gcp.audit.resource.labels.bucket_name`.  
    - **Cloud Monitoring**: Configura una métrica basada en logs para `methodName="storage.objects.create"` y establece alertas para picos.

- **🗑️ `storage.objects.delete`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Investigar pérdidas de datos o eliminaciones maliciosas en Cloud Storage.  
  - **Query**:  
    - **Cloud Logging**: En Log Explorer, usa `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="storage.objects.delete"`. Filtra por `principalEmail` o `bucket_name`.  
    - **Chronicle**: Busca `event.metadata.event_type="STORAGE_OBJECT_DELETE"` y `event.target.resource.resource_type="GCS_BUCKET"`.  
    - **Splunk**: Filtra por `sourcetype="google:gcp:audit" methodName="storage.objects.delete"` con campos como `principalEmail`.  
    - **Elastic**: Usa `gcp.audit.methodName:"storage.objects.delete"` en Kibana, filtrando por `gcp.audit.resource.labels.bucket_name`.  
    - **Cloud Monitoring**: Crea una métrica basada en logs para `methodName="storage.objects.delete"` y configura alertas.

- **🔧 `storage.buckets.update`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Rastrear cambios en permisos de buckets que puedan exponer datos (e.g., volverse públicos).  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="storage.buckets.update"`. Revisa `protoPayload.authorizationInfo` para permisos modificados.  
    - **Chronicle**: Busca `event.metadata.event_type="STORAGE_BUCKET_UPDATE"` con `event.target.resource.resource_type="GCS_BUCKET"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="storage.buckets.update"` y filtra por `authorizationInfo`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"storage.buckets.update"` en Kibana, con condiciones para `gcp.audit.authorizationInfo`.  
    - **Cloud Monitoring**: Configura una métrica basada en logs para `methodName="storage.buckets.update"` y alerta sobre cambios.

- **🔑 `iam.serviceAccounts.create`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar creación de cuentas de servicio que puedan usarse para escalar privilegios.  
  - **Query**:  
    - **Cloud Logging**: Usa `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="google.iam.admin.v1.CreateServiceAccount"`. Filtra por `principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="IAM_SERVICE_ACCOUNT_CREATE"`.  
    - **Splunk**: Filtra por `sourcetype="google:gcp:audit" methodName="google.iam.admin.v1.CreateServiceAccount"`.  
    - **Elastic**: Usa `gcp.audit.methodName:"google.iam.admin.v1.CreateServiceAccount"` en Kibana.  
    - **Cloud Monitoring**: Crea una métrica para `methodName="google.iam.admin.v1.CreateServiceAccount"` y configura alertas.

- **🔒 `iam.serviceAccounts.setIamPolicy`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Investigar cambios en permisos de cuentas de servicio que permitan accesos no autorizados.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="SetIamPolicy" serviceName="iam.googleapis.com"`. Revisa `authorizationInfo`.  
    - **Chronicle**: Busca `event.metadata.event_type="IAM_SET_POLICY" event.target.resource.resource_type="IAM_SERVICE_ACCOUNT"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="SetIamPolicy" serviceName="iam.googleapis.com"`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"SetIamPolicy" gcp.audit.serviceName:"iam.googleapis.com"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="SetIamPolicy" serviceName="iam.googleapis.com"` y alerta.

- **💻 `compute.instances.insert`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar despliegues sospechosos (e.g., instancias para criptominería).  
  - **Query**:  
    - **Cloud Logging**: Usa `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="compute.instances.insert"`. Filtra por `principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="COMPUTE_INSTANCE_CREATE"`.  
    - **Splunk**: Filtra por `sourcetype="google:gcp:audit" methodName="compute.instances.insert"`.  
    - **Elastic**: Usa `gcp.audit.methodName:"compute.instances.insert"` en Kibana.  
    - **Cloud Monitoring**: Crea una métrica para `methodName="compute.instances.insert"` y configura alertas.

- **🗑️ `compute.instances.delete`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Investigar eliminaciones de instancias que causen interrupciones o sabotajes.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="compute.instances.delete"`. Revisa `resource.labels.instance_id`.  
    - **Chronicle**: Busca `event.metadata.event_type="COMPUTE_INSTANCE_DELETE"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="compute.instances.delete"`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"compute.instances.delete"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="compute.instances.delete"` y alerta.

- **🔥 `compute.firewalls.update`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar cambios en reglas de firewall que permitan accesos no autorizados.  
  - **Query**:  
    - **Cloud Logging**: Usa `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="compute.firewalls.update"`. Filtra por `resource.labels.firewall_rule_id`.  
    - **Chronicle**: Busca `event.metadata.event_type="COMPUTE_FIREWALL_UPDATE"`.  
    - **Splunk**: Filtra por `sourcetype="google:gcp:audit" methodName="compute.firewalls.update"`.  
    - **Elastic**: Usa `gcp.audit.methodName:"compute.firewalls.update"` en Kibana.  
    - **Cloud Monitoring**: Crea una métrica para `methodName="compute.firewalls.update"` y configura alertas.

- **📊 `bigquery.datasets.get`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos a metadatos de datasets en BigQuery que puedan indicar exploración no autorizada.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="google.cloud.bigquery.v2.DatasetService.GetDataset"`. Revisa `principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="BIGQUERY_DATASET_GET"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="google.cloud.bigquery.v2.DatasetService.GetDataset"`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"google.cloud.bigquery.v2.DatasetService.GetDataset"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="google.cloud.bigquery.v2.DatasetService.GetDataset"` y alerta.

- **🔍 `bigquery.jobs.create`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Investigar consultas en BigQuery que puedan extraer datos sensibles.  
  - **Query**:  
    - **Cloud Logging**: Usa `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="google.cloud.bigquery.v2.JobService.InsertJob"`. Filtra por `principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="BIGQUERY_JOB_CREATE"`.  
    - **Splunk**: Filtra por `sourcetype="google:gcp:audit" methodName="google.cloud.bigquery.v2.JobService.InsertJob"`.  
    - **Elastic**: Usa `gcp.audit.methodName:"google.cloud.bigquery.v2.JobService.InsertJob"` en Kibana.  
    - **Cloud Monitoring**: Crea una métrica para `methodName="google.cloud.bigquery.v2.JobService.InsertJob"` y configura alertas.

- **📨 `pubsub.subscriptions.create`**  
  - **Evento**: Admin Activity Log  
  - **Relevancia**: Detectar creación de suscripciones en Pub/Sub que puedan usarse para exfiltrar datos.  
  - **Query**:  
    - **Cloud Logging**: Filtra por `logName:"/logs/cloudaudit.googleapis.com%2Factivity"` y `protoPayload.methodName="google.pubsub.v1.Subscriber.CreateSubscription"`. Revisa `resource.labels.subscription_id`.  
    - **Chronicle**: Busca `event.metadata.event_type="PUBSUB_SUBSCRIPTION_CREATE"`.  
    - **Splunk**: Usa `sourcetype="google:gcp:audit" methodName="google.pubsub.v1.Subscriber.CreateSubscription"`.  
    - **Elastic**: Filtra por `gcp.audit.methodName:"google.pubsub.v1.Subscriber.CreateSubscription"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="google.pubsub.v1.Subscriber.CreateSubscription"` y alerta.

- **📝 `logging.logEntries.create`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Investigar creación manual de entradas de log que pueda ocultar actividades maliciosas.  
  - **Query**:  
    - **Cloud Logging**: Usa `logName:"/logs/cloudaudit.googleapis.com%2Fdata_access"` y `protoPayload.methodName="google.logging.v2.LoggingServiceV2.WriteLogEntries"`. Filtra por `principalEmail`.  
    - **Chronicle**: Busca `event.metadata.event_type="LOGGING_LOG_ENTRIES_CREATE"`.  
    - **Splunk**: Filtra por `sourcetype="google:gcp:audit" methodName="google.logging.v2.LoggingServiceV2.WriteLogEntries"`.  
    - **Elastic**: Usa `gcp.audit.methodName:"google.logging.v2.LoggingServiceV2.WriteLogEntries"` en Kibana.  
    - **Cloud Monitoring**: Configura una métrica para `methodName="google.logging.v2.LoggingServiceV2.WriteLogEntries"` y configura alertas.

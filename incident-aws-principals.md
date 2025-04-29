## 🔍 Investigación de Incidentes de Seguridad en AWS CloudTrail

- **📁 Pérdida Crítica de Datos en S3: Evento de Eliminación Inesperada de Bucket**  
  - **Acción de Evento**: `DeleteBucket`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar eliminaciones no autorizadas de buckets S3 que causen pérdida de datos críticos.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="s3.amazonaws.com"` y `eventName="DeleteBucket"`. Revisa `userIdentity.arn` y `requestParameters.bucketName` para identificar el responsable y el bucket.  
    - **AWS Security Hub**: Busca hallazgos relacionados con `s3:DeleteBucket` en el panel de Findings.  
    - **Splunk**: Configura un filtro para `sourcetype="aws:cloudtrail" eventName="DeleteBucket"`, usando `userIdentity.arn` y `requestParameters.bucketName`.  
    - **Elastic**: En Kibana, filtra por `aws.cloudtrail.eventName:"DeleteBucket"`, con condiciones para `aws.cloudtrail.userIdentity.arn`.  
    - **CloudWatch Metrics**: Crea una métrica basada en logs para `eventName="DeleteBucket"` y configura alertas para eliminaciones.

- **🔑 Aprovisionamiento Sospechoso de Perfil de Instancia IAM**  
  - **Acción de Evento**: `CreateInstanceProfile`  
  - **Evento**: Management Event  
  - **Relevancia**: Investigar creación de perfiles de instancia IAM que otorguen permisos indebidos a instancias EC2.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="iam.amazonaws.com"` y `eventName="CreateInstanceProfile"`. Revisa `userIdentity.arn` y `requestParameters.instanceProfileName`.  
    - **AWS Security Hub**: Busca hallazgos para `iam:CreateInstanceProfile` en Findings.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="CreateInstanceProfile"`, con `userIdentity.arn`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"CreateInstanceProfile"` en Kibana, con `aws.cloudtrail.requestParameters.instanceProfileName`.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="CreateInstanceProfile"` y configura alertas.

- **🔒 Modificación No Autorizada de Políticas IAM Detectada**  
  - **Acción de Evento**: `UpdateRolePolicy`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar cambios en permisos de roles IAM que permitan accesos no autorizados.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="iam.amazonaws.com"` y `eventName="UpdateRolePolicy"`. Revisa `requestParameters.roleName` y `userIdentity.arn`.  
    - **AWS Security Hub**: Busca hallazgos para `iam:UpdateRolePolicy` en Findings.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="UpdateRolePolicy"`, con `requestParameters.roleName`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"UpdateRolePolicy"` en Kibana, con `aws.cloudtrail.userIdentity.arn`.  
    - **CloudWatch Metrics**: Configura una métrica para `eventName="UpdateRolePolicy"` y alerta.

- **🛠️ Ejecución Sospechosa de Función Lambda Identificada**  
  - **Acción de Evento**: `Invoke`  
  - **Evento**: Data Event  
  - **Relevancia**: Investigar invocaciones no autorizadas de funciones Lambda que puedan ejecutar código malicioso.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="lambda.amazonaws.com"` y `eventName="Invoke"`. Revisa `userIdentity.arn` y `requestParameters.functionName`.  
    - **AWS Security Hub**: Busca hallazgos para `lambda:Invoke` en Findings.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="Invoke"`, con `requestParameters.functionName`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"Invoke"` en Kibana, con `aws.cloudtrail.requestParameters.functionName`.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="Invoke"` y configura alertas para invocaciones anómalas.

- **🤝 Relación de Confianza Cruzada Sospechosa Identificada**  
  - **Acción de Evento**: `CreateRole`  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar creación de roles IAM con relaciones de confianza cruzadas que permitan accesos externos no autorizados.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="iam.amazonaws.com"` y `eventName="CreateRole"`. Revisa `requestParameters.trustPolicy` para cuentas externas en `userIdentity.accountId`.  
    - **AWS Security Hub**: Busca hallazgos para `iam:CreateRole` con políticas de confianza sospechosas.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="CreateRole"`, filtrando por `requestParameters.trustPolicy`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"CreateRole"` en Kibana, con `aws.cloudtrail.requestParameters.trustPolicy`.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="CreateRole"` y alerta si `trustPolicy` incluye cuentas externas.

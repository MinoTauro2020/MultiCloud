## 🔍 Event Actions Interesantes para Investigación de Incidentes en AWS CloudTrail

- **🗂️ `GetObject`**  
  - **Evento**: Data Event  
  - **Relevancia**: Rastrear accesos no autorizados o extracción de datos sensibles en S3.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="s3.amazonaws.com"` y `eventName="GetObject"`. Agrega condiciones para `userIdentity.arn` o `resources.ARN` para usuario o bucket específico.  
    - **AWS Security Hub**: Busca hallazgos relacionados con `s3:GetObject` en el panel de Findings, correlacionando con CloudTrail logs.  
    - **Splunk**: Configura un filtro para `sourcetype="aws:cloudtrail" eventName="GetObject"`, usando campos como `userIdentity.arn`.  
    - **Elastic**: En Kibana, filtra por `aws.cloudtrail.eventName:"GetObject"`, con condiciones para `aws.cloudtrail.userIdentity.arn`.  
    - **CloudWatch Metrics**: Crea una métrica basada en logs para `eventName="GetObject"` y configura alertas para accesos anómalos.

- **📤 `PutObject`**  
  - **Evento**: Data Event  
  - **Relevancia**: Detectar cargas sospechosas o subidas de malware a buckets S3.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="s3.amazonaws.com"` y `eventName="PutObject"`. Incluye `resources.ARN` para el bucket.  
    - **AWS Security Hub**: Revisa hallazgos para `s3:PutObject` en Findings.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="PutObject"` y filtra por `resources.ARN`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"PutObject"` en Kibana, con `aws.cloudtrail.resources.ARN`.  
    - **CloudWatch Metrics**: Configura una métrica para `eventName="PutObject"` y alerta sobre picos.

- **🗑️ `DeleteObject`**  
  - **Evento**: Data Event  
  - **Relevancia**: Investigar eliminaciones maliciosas o accidentales de objetos en S3.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="s3.amazonaws.com"` y `eventName="DeleteObject"`. Revisa `userIdentity.arn`.  
    - **AWS Security Hub**: Busca hallazgos para `s3:DeleteObject`.  
    - **Splunk**: Filtra por `sourcetype="aws:cloudtrail" eventName="DeleteObject"`, con `userIdentity.arn`.  
    - **Elastic**: Usa `aws.cloudtrail.eventName:"DeleteObject"` en Kibana, filtrando por `aws.cloudtrail.userIdentity.arn`.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="DeleteObject"` y configura alertas.

- **🔧 `PutBucketPolicy`**  
  - **Evento**: Management Event  
  - **Relevancia**: Rastrear cambios en políticas de buckets S3 que puedan exponer datos.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="s3.amazonaws.com"` y `eventName="PutBucketPolicy"`. Revisa `requestParameters.bucketName`.  
    - **AWS Security Hub**: Busca hallazgos para `s3:PutBucketPolicy` en Findings.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="PutBucketPolicy"`, filtrando por `requestParameters.bucketName`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"PutBucketPolicy"` en Kibana, con `aws.cloudtrail.requestParameters.bucketName`.  
    - **CloudWatch Metrics**: Configura una métrica para `eventName="PutBucketPolicy"` y alerta.

- **🔑 `CreateRole`**  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar creación de roles IAM que puedan usarse para escalar privilegios.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="iam.amazonaws.com"` y `eventName="CreateRole"`. Revisa `userIdentity.arn`.  
    - **AWS Security Hub**: Busca hallazgos para `iam:CreateRole`.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="CreateRole"`, con `userIdentity.arn`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"CreateRole"` en Kibana.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="CreateRole"` y configura alertas.

- **🔒 `UpdateRolePolicy`**  
  - **Evento**: Management Event  
  - **Relevancia**: Investigar cambios en permisos de roles IAM que permitan accesos no autorizados.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="iam.amazonaws.com"` y `eventName="UpdateRolePolicy"`. Revisa `requestParameters.roleName`.  
    - **AWS Security Hub**: Busca hallazgos para `iam:UpdateRolePolicy`.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="UpdateRolePolicy"`, con `requestParameters.roleName`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"UpdateRolePolicy"` en Kibana.  
    - **CloudWatch Metrics**: Configura una métrica para `eventName="UpdateRolePolicy"` y alerta.

- **💻 `RunInstances`**  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar creación de instancias EC2 sospechosas (e.g., criptominería).  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="ec2.amazonaws.com"` y `eventName="RunInstances"`. Revisa `userIdentity.arn`.  
    - **AWS Security Hub**: Busca hallazgos para `ec2:RunInstances`.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="RunInstances"`, con `userIdentity.arn`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"RunInstances"` en Kibana.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="RunInstances"` y configura alertas.

- **🗑️ `TerminateInstances`**  
  - **Evento**: Management Event  
  - **Relevancia**: Investigar eliminaciones de instancias EC2 que causen interrupciones.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="ec2.amazonaws.com"` y `eventName="TerminateInstances"`. Revisa `requestParameters.instanceId`.  
    - **AWS Security Hub**: Busca hallazgos para `ec2:TerminateInstances`.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="TerminateInstances"`, con `requestParameters.instanceId`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"TerminateInstances"` en Kibana.  
    - **CloudWatch Metrics**: Configura una métrica para `eventName="TerminateInstances"` y alerta.

- **🔥 `AuthorizeSecurityGroupIngress`**  
  - **Evento**: Management Event  
  - **Relevancia**: Detectar cambios en reglas de seguridad que permitan accesos no autorizados.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="ec2.amazonaws.com"` y `eventName="AuthorizeSecurityGroupIngress"`. Revisa `requestParameters.groupId`.  
    - **AWS Security Hub**: Busca hallazgos para `ec2:AuthorizeSecurityGroupIngress`.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="AuthorizeSecurityGroupIngress"`, con `requestParameters.groupId`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"AuthorizeSecurityGroupIngress"` en Kibana.  
    - **CloudWatch Metrics**: Configura una métrica para `eventName="AuthorizeSecurityGroupIngress"` y alerta.

- **🔍 `ConsoleLogin`**  
  - **Evento**: Management Event  
  - **Relevancia**: Rastrear inicios de sesión sospechosos en la consola de AWS.  
  - **Query**:  
    - **CloudWatch Logs Insights**: Filtra por `eventSource="signin.amazonaws.com"` y `eventName="ConsoleLogin"`. Revisa `additionalEventData.MFAUsed` y `sourceIPAddress`.  
    - **AWS Security Hub**: Busca hallazgos para `signin:ConsoleLogin`.  
    - **Splunk**: Usa `sourcetype="aws:cloudtrail" eventName="ConsoleLogin"`, con `additionalEventData.MFAUsed`.  
    - **Elastic**: Filtra por `aws.cloudtrail.eventName:"ConsoleLogin"` en Kibana, con `aws.cloudtrail.additionalEventData.MFAUsed`.  
    - **CloudWatch Metrics**: Crea una métrica para `eventName="ConsoleLogin"` y alerta si `MFAUsed="No"`.


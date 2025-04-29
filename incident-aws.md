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

## 🔍 Event Actions Interesantes para Investigación de Incidentes en Azure Audit Logs

- **🔑 `Microsoft.Authorization/roleAssignments/write`**  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar creación de asignaciones de roles que otorguen permisos elevados.  
  - **Query**:  
    - **Azure Monitor Logs**: En Log Analytics, filtra por `Category="Administrative"` y `OperationName="Create role assignment"`. Agrega condiciones para `Caller` o `Properties.RoleDefinitionId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Authorization/roleAssignments/write` en el panel de Incidents.  
    - **Splunk**: Configura un filtro para `sourcetype="azure:audit" OperationName="Create role assignment"`, usando `Caller`.  
    - **Elastic**: En Kibana, filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Create role assignment"`.  
    - **Azure Monitor Metrics**: Crea una métrica basada en logs para `OperationName="Create role assignment"` y configura alertas.

- **🔒 `Microsoft.Authorization/roleAssignments/delete`**  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar eliminaciones de asignaciones de roles que puedan afectar accesos legítimos.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Delete role assignment"`. Revisa `Caller`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Authorization/roleAssignments/delete`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Delete role assignment"`, con `Caller`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Delete role assignment"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Delete role assignment"` y alerta.

- **💻 `Microsoft.Compute/virtualMachines/write`**  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar creación de máquinas virtuales sospechosas (e.g., criptominería).  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Create or Update Virtual Machine"`. Revisa `ResourceId` y `Caller`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Compute/virtualMachines/write`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Create or Update Virtual Machine"`, con `ResourceId`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Create or Update Virtual Machine"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Create or Update Virtual Machine"` y alerta.

- **🗑️ `Microsoft.Compute/virtualMachines/delete`**  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar eliminaciones de VMs que causen interrupciones.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Delete Virtual Machine"`. Revisa `ResourceId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Compute/virtualMachines/delete`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Delete Virtual Machine"`, con `ResourceId`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Delete Virtual Machine"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Delete Virtual Machine"` y alerta.

- **🗂️ `Microsoft.Storage/storageAccounts/blobServices/blobs/read`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a blobs en Azure Storage.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Audit"` y `OperationName="Get Blob"`. Revisa `CallerIpAddress` y `ResourceId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Storage/storageAccounts/blobServices/blobs/read`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Get Blob"`, con `CallerIpAddress`.  
    - **Elastic**: Filtra por `azure.audit.category:"Audit" azure.audit.operationName:"Get Blob"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Get Blob"` y alerta.

- **📤 `Microsoft.Storage/storageAccounts/blobServices/blobs/write`**  
  - **Evento**: Data Access Log  
  - **Relevancia**: Detectar cargas sospechosas a blobs en Azure Storage.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Audit"` y `OperationName="Put Blob"`. Revisa `CallerIpAddress`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Storage/storageAccounts/blobServices/blobs/write`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Put Blob"`, con `CallerIpAddress`.  
    - **Elastic**: Filtra por `azure.audit.category:"Audit" azure.audit.operationName:"Put Blob"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Put Blob"` y alerta.

- **🔍 `Microsoft.Storage/storageAccounts/delete`**  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar eliminaciones de cuentas de almacenamiento que causen pérdida de datos.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Delete Storage Account"`. Revisa `ResourceId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Storage/storageAccounts/delete`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Delete Storage Account"`, con `ResourceId`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Delete Storage Account"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Delete Storage Account"` y alerta.

- **🔐 `Microsoft.Network/networkSecurityGroups/write`**  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar cambios en reglas de NSG que permitan accesos no autorizados.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Create or Update Network Security Group"`. Revisa `ResourceId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Network/networkSecurityGroups/write`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Create or Update Network Security Group"`, con `ResourceId`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Create or Update Network Security Group"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Create or Update Network Security Group"` y alerta.

- **🔑 `Microsoft.AAD/Directory/SignIn`**  
  - **Evento**: Sign-In Log  
  - **Relevancia**: Rastrear inicios de sesión sospechosos en Azure AD.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="SignInLogs"` y `ResultType="0" (success) o ResultType!="0" (failed)`. Revisa `IPAddress` y `ConditionalAccessStatus`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.AAD/Directory/SignIn` en Incidents, enfocándote en `RiskLevel`.  
    - **Splunk**: Usa `sourcetype="azure:aad:signin"`, filtrando por `ResultType` y `IPAddress`.  
    - **Elastic**: Filtra por `azure.signinlogs.properties.resultType:*` en Kibana, con `azure.signinlogs.properties.ipAddress`.  
    - **Azure Monitor Metrics**: Crea una métrica para `Category="SignInLogs"` y alerta si `ConditionalAccessStatus="failure"`.

- **🚨 `Microsoft.Security/alerts/write`**  
  - **Evento**: Security Log  
  - **Relevancia**: Investigar alertas de seguridad generadas por Microsoft Defender for Cloud.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Security"` y `OperationName="Create or Update Alert"`. Revisa `Properties.AlertSeverity`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Security/alerts/write` en Incidents.  
    - **Splunk**: Usa `sourcetype="azure:security" OperationName="Create or Update Alert"`, con `Properties.AlertSeverity`.  
    - **Elastic**: Filtra por `azure.security.category:"Security" azure.security.operationName:"Create or Update Alert"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Create or Update Alert"` y alerta para alta severidad.

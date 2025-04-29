## 🔍 Acciones de Eventos Interesantes para Investigación de Incidentes en Azure Audit Logs

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
  - **Relevancia**: Investigar eliminaciones de asignaciones de roles que afecten accesos legítimos.  
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
    - **Azure Monitor Logs**: Filtra por `Category="SignInLogs"` y `ResultType="0" (éxito) o ResultType!="0" (fallo)`. Revisa `IPAddress` y `ConditionalAccessStatus`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.AAD/Directory/SignIn` en el panel de Incidents, enfocándote en `RiskLevel`.  
    - **Splunk**: Usa `sourcetype="azure:aad:signin"`, filtrando por `ResultType` y `IPAddress`.  
    - **Elastic**: Filtra por `azure.signinlogs.properties.resultType:*` en Kibana, con `azure.signinlogs.properties.ipAddress`.  
    - **Azure Monitor Metrics**: Crea una métrica para `Category="SignInLogs"` y alerta si `ConditionalAccessStatus="failure"`.

- **🚨 `Microsoft.Security/alerts/write`**  
  - **Evento**: Security Log  
  - **Relevancia**: Investigar alertas de seguridad generadas por Microsoft Defender for Cloud.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Security"` y `OperationName="Create or Update Alert"`. Revisa `Properties.AlertSeverity`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Security/alerts/write` en el panel de Incidents.  
    - **Splunk**: Usa `sourcetype="azure:security" OperationName="Create or Update Alert"`, con `Properties.AlertSeverity`.  
    - **Elastic**: Filtra por `azure.security.category:"Security" azure.security.operationName:"Create or Update Alert"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Create or Update Alert"` y alerta para alta severidad.

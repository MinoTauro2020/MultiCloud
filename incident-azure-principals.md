## 🔍 Investigación de Incidentes de Seguridad en Azure Audit Logs

- **🔍 Actividad Inusual de Solicitud LISTKEYS Registrada**  
  - **Acción de Evento**: `Microsoft.Storage/storageAccounts/listKeys/action`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar solicitudes para listar claves de almacenamiento que indiquen intentos de acceso no autorizado.  
  - **Query**:  
    - **Azure Monitor Logs**: En Log Analytics, filtra por `Category="Administrative"` y `OperationName="List Storage Account Keys"`. Revisa `CallerIpAddress` y `ResourceId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Storage/storageAccounts/listKeys/action` en el panel de Incidents.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="List Storage Account Keys"`, con `CallerIpAddress`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"List Storage Account Keys"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="List Storage Account Keys"` y alerta para actividad inusual.

- **🔐 Configuración No Aprobada de Grupo de Seguridad de Red Detectada**  
  - **Acción de Evento**: `Microsoft.Network/networkSecurityGroups/write`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Detectar cambios en reglas de NSG que permitan accesos no autorizados.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Create or Update Network Security Group"`. Revisa `ResourceId` y `Caller`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.Network/networkSecurityGroups/write` en Incidents.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Create or Update Network Security Group"`, con `ResourceId`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Create or Update Network Security Group"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Create or Update Network Security Group"` y alerta.

- **📦 Enumeración de Recursos de Contenedores de Azure**  
  - **Acción de Evento**: `Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar solicitudes para listar credenciales de clústeres AKS que indiquen intentos de enumeración no autorizada.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="List Cluster Admin Credential"`. Revisa `CallerIpAddress` y `ResourceId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action`.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="List Cluster Admin Credential"`, con `CallerIpAddress`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"List Cluster Admin Credential"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="List Cluster Admin Credential"` y alerta.

- **🔐 Posible Exfiltración de Secretos desde Azure Key Vault Identificada**  
  - **Acción de Evento**: `Microsoft.KeyVault/vaults/secrets/read`  
  - **Evento**: Data Access Log  
  - **Relevancia**: Rastrear accesos no autorizados a secretos en Key Vault que indiquen exfiltración.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Audit"` y `OperationName="SecretGet"`. Revisa `CallerIpAddress` y `Properties.SecretName`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.KeyVault/vaults/secrets/read` en Incidents.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="SecretGet"`, con `CallerIpAddress`.  
    - **Elastic**: Filtra por `azure.audit.category:"Audit" azure.audit.operationName:"SecretGet"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="SecretGet"` y alerta para accesos anómalos.

- **🔑 Actualización Sospechosa de Credenciales de Registro de Aplicaciones Detectada**  
  - **Acción de Evento**: `Microsoft.AAD/applications/credentials/update`  
  - **Evento**: Administrative Log  
  - **Relevancia**: Investigar cambios en credenciales de aplicaciones que permitan accesos no autorizados.  
  - **Query**:  
    - **Azure Monitor Logs**: Filtra por `Category="Administrative"` y `OperationName="Update application - Certificates and secrets management"`. Revisa `Caller` y `Properties.AppId`.  
    - **Azure Sentinel**: Busca alertas para `Microsoft.AAD/applications/credentials/update` en Incidents.  
    - **Splunk**: Usa `sourcetype="azure:audit" OperationName="Update application - Certificates and secrets management"`, con `Properties.AppId`.  
    - **Elastic**: Filtra por `azure.audit.category:"Administrative" azure.audit.operationName:"Update application - Certificates and secrets management"`.  
    - **Azure Monitor Metrics**: Configura una métrica para `OperationName="Update application - Certificates and secrets management"` y alerta.

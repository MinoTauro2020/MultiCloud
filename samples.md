## 🔍 Ejercicios de Investigación de Incidentes en AWS con Queries Simplificadas para Kibana Discover

1. Determinar el conteo de intentos de enumeración de subdominios detectados contra el bucket S3 público `bucket666`
 Contar los intentos de enumeración de subdominios contra el bucket S3 `bucket666`, identificando patrones de herramientas como Wfuzz.

event.provider:s3.amazonaws.com AND event.action:GetObject AND event.outcome:success

3. Identificar y nombrar los objetos del bucket descubiertos exitosamente mediante enumeración de subdominios por un atacante
Encontrar los objetos del bucket appbackupfilesbuk que fueron accedidos exitosamente usando Wfuzz.

event.provider:s3.amazonaws.com AND event.action:GetObject AND event.outcome:success

4. Identificar y determinar el roleArn que fue asumido exitosamente
Determinar el ARN del rol IAM que fue asumido mediante la acción AssumeRole.

event.provider:sts.amazonaws.com AND event.action:AssumeRole AND event.outcome:success

6. Determinar el nombre del perfil de instancia asociado con un evento de creación sospechoso
Descripción: Identificar el nombre del perfil de instancia creado mediante la acción CreateInstanceProfile.

event.provider:iam.amazonaws.com AND event.action:CreateInstanceProfile AND event.outcome:success

7. Identificar el nombre del rol IAM asociado con el evento AddRoleToInstanceProfile
Descripción: Determinar el nombre del rol IAM agregado a un perfil de instancia mediante la acción AddRoleToInstanceProfile.
event.provider:iam.amazonaws.com AND event.action:AddRoleToInstanceProfile AND event.outcome:success

6. Recuperar el user_identity.arn asociado con la actividad de generación de claves de acceso
Identificar el ARN del usuario o rol que generó una clave de acceso mediante la acción CreateAccessKey.

event.provider:iam.amazonaws.com AND event.action:CreateAccessKey AND event.outcome:success

8. Identificar el nombre específico de la función usado durante la solicitud CreateFunction
Determinar el nombre de la función Lambda creada mediante la acción CreateFunction.

event.provider:lambda.amazonaws.com AND event.action:CreateFunction AND event.outcome:success

10. Determinar el valor de secretId asociado con el evento GetSecretValue
Identificar el identificador del secreto accedido mediante la acción GetSecretValue en Secrets Manager.

event.provider:secretsmanager.amazonaws.com AND event.action:GetSecretValue AND event.outcome:success

12. Analizar y extraer el nombre del bucket de almacenamiento asociado con el evento de eliminación
Determinar el nombre del bucket S3 eliminado mediante la acción DeleteBucket.

event.provider:s3.amazonaws.com AND event.action:DeleteBucket AND event.outcome:success

11. Identificar el nombre del CloudTrail vinculado a la actividad StopLogging
 Determinar el nombre del trail de CloudTrail detenido mediante la acción StopLogging.

event.provider:cloudtrail.amazonaws.com AND event.action:StopLogging AND event.outcome:success


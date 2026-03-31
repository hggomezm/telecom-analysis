# telecom-analysis (CONNECTATEL)
Este análisis es de una empresa de telecomunicaciones con operaciones en México y Colombia. Se busca analizar el uso de los servicios moviles. Pero primero se debe explorar, limpiar y analizar estas bases de datos para construir una visión clara y confiable sobre el comportamiento de los usuarios. Se cuenta con 3 data sets de los cuáles sacaremos información:
plans.csv → información de los planes actuales (precio, minutos incluidos, GB incluidos, costo por extra).
users.csv → información de los clientes (edad, ciudad, fecha de registro, plan, churn)
usage.csv → detalle del uso real de los servicios (llamadas y mensajes)

¿Qué problemas se encontraron inicialmente en los datos? ¿qué tratamiento se les dió?

users.csv:
El tipo de dato de la columna 'reg_date' se debía de cambiar a tipo fecha, las fechas futuras, superiores a 2024 cambiarlas a NA, ya que los datos con los que contábamos eran solo hasta el 2024, por lo que cualquier otra fecha posterior a ese año es considerada como error de dedo.
Columna 'city' tiene 11% de nulos, se revisa la relación entre 'city' y 'user_id', y se eliminan las filas que no cuentan con la ciudad ya que imputar no es una opción.
Revisión de Sentinels: La columna 'user_id' no tiene sentinels. La columna 'age' sí tiene 55 veces el sentinel -999, el cual es reemplazado por la mediana. En el caso de las columnas categóricas, se corrige el tipo de sentinels de la columna 'city', que tiene 96 valores del sentinel '?', que se reemplaza por "NA" .
La columna churn_date tiene 88% de nulos, lo cuál es bueno ya que esto indica que hay un 88% de retención y solo un 12% de los usuarios han finalizado su contrato.

usage.csv:
El tipo de dato de la columna 'date' se debía cambiar a tipo fecha. Como se mencionó, en el DF solo se encuentran datos del año 2024. Esta columna tiene un 1% de nulos que dejarlos como nulo.
La columna 'duration' tiene 55% de nulos, el cual esta relacionado con a la duración del texto; en ese caso se deja como nulo, puesto que un texto no se mide por duración de tiempo, sino por caracteres, es decir, que son de tipo MAR, que depende del 'type'.
La columna length tiene 44% de nulos, que esta relacionado con la elongación de caracteres de las llamadas, en ese caso tiene sentido dejarlo como nulo, puesto que una llamada no se mide por carácter sino por duración. Es decir que, son de tipo MAR que depende del 'type'.
Por ende dado que el 100% de los nulos en duration eran de Text y el 100% de los nulos en Length eran de Llamadas, hace sentido dejarlos así.
Para la revision de sentinels: La columna 'user_id' y 'Id' no tiene sentinels detectados, en cambio las columnas 'duration' y 'length' se encontró 15 y 133 sentinels respectivamente para el tipo 0.0000, puedo dejarlos como error.
¿Qué segmentos de clientes identificaste y cómo se comportan según su edad y nivel de uso?

Se encuentra que el 64.8% de los usuarios tiene el plan Básico y 35.2% el plan Premium. En ambos planes se encuentra un comportamiento similar, en el cual recorre por todas las edades desde 18 años hasta los 80 años, pero en su mayoria son personas adultas entre 30 y 60 años. Los usuarios en su mayoria tienen un uso moderado de llamdas y mensajes de texto.
Se solicitó dividir en 3 grupos diferentes de edad y 3 grupos diferentes de uso. Básicamente los el sector que tiene mayor cantidad de usuarios es el de adultos de 30 a 59 años y el que menos usuarios tiene es el de jovenes, menores a 30 años. Por otra parte parece que la mayoría de los usuarios están en el segmento de alto uso, sin embargo, creo que las instrucciones están mal, ya que nos pide identificar como bajo uso todo lo que sea menor de 5 llamadas y 5 mensajes, medio uso lo que esté entre 5-9 llamadas y 5-9 mensajes y todo lo demás alto uso. Sin embargo, hay un error ya que el sistema en caso de que por ejemplo un usuario tenga 3 mensajes y 7 llamadas, lo considera como alto uso porque no está dentro de los rangos de bajo y medio uso ya que no cumple las dos condiciones solicitadas. Pero ahí dice muy claro que tienen que ser ambas y que todo lo demás se considere Alto Uso.  

➡️ Esto sugiere que hay que aclarar la instrucción para que no se vea afectado el segmento por nivel de uso y poner gran enfoque en como atrapar más al segmento de jovenes.

¿Qué segmentos parecen más valiosos para ConnectaTel y por qué? Dfinitivamente el sector más valioso para ConnectaTel actualmente es el de los Adultos con plan básico, por la cantidad de usuarios que tienen y además un buen porcentage cuenta con el plan Premium. Por otra parte, en el segmento de los Adulto Mayores, hay un secotr (de 72 a 79 años aprox.) que casi iguala la cantidad de usuarios Premium con Básico, lo cuál hace que sea un segmento valioso también.
- ¿Qué patrones de uso extremo (outliers) encontraste y qué implican para el negocio?
Se detectaron muy pocos Outliers en la cantidad de llamadas y de mensajes, nomás en donde si hubo más Outliers fue en la cantidad de minutos por llamada. Las implicaciones es que nos deja ver que prácticamente el plan Básico cubre la nececidad promedio de casi todos los clientes. Con sus 100 mensajes y 100 minutos incluidos, son muy pocos los usuarios que requieren más minutos. 

¿Qué patrones de uso extremo (outliers) encontraste y qué implican para el negocio?

Se detectaron muy pocos Outliers en la cantidad de llamadas y de mensajes, nomás en donde si hubo más Outliers fue en la cantidad de minutos por llamada. Las implicaciones es que nos deja ver que prácticamente el plan Básico cubre la nececidad promedio de casi todos los clientes. Con sus 100 mensajes y 100 minutos incluidos, son muy pocos los usuarios que requieren más minutos. 

Análisis ejecutivo
⚠️ Problemas detectados en los datos

En ambos datasets la columna relacionada a las fechas estaba en formato object, el cual se cambia a tipo fecha.
Se encuentran varios tipos de Sentinel, de tipo numérico y categórico. A los cuales se les realiza su respectivo tratamiento. Se cambian a su valor promedio, se cambian a NAN.

🔍 Segmentos por Edad y 📊 Segmentos por Nivel de Uso

Se solicitó dividir en 3 grupos diferentes de edad y 3 grupos diferentes de uso. Básicamente los el sector que tiene mayor cantidad de usuarios es el de adultos de 30 a 59 años y el que menos usuarios tiene es el de jovenes, menores a 30 años. Por otra parte parece que la mayoría de los usuarios están en el segmento de alto uso, sin embargo, creo que las instrucciones están mal, ya que nos pide identificar como bajo uso todo lo que sea menor de 5 llamadas y 5 mensajes, medio uso lo que esté entre 5-9 llamadas y 5-9 mensajes y todo lo demás alto uso. Sin embargo, hay un error ya que el sistema en caso de que por ejemplo un usuario tenga 3 mensajes y 7 llamadas, lo considera como alto uso porque no está dentro de los rangos de bajo y medio uso ya que no cumple las dos condiciones solicitadas. Pero ahí dice muy claro que tienen que ser ambas y que todo lo demás se considere Alto Uso.  

➡️ Esto sugiere que hay que aclarar la instrucción para que no se vea afectado el segmento por nivel de uso y poner gran enfoque en como atrapar más al segmento de jovenes. Una opción pudiera ser paquetes con algunas redes sociales ilimitadas.

💡 Recomendaciones

Me tomé la libertad de hace otra columna con grupo_uso2, en donde para estár en Alto uso tenía que cumplir las dos condiciones es decir enviar mínimo 10 mensajes y realizar mínimo 10 llamadas. Para Uso Medio, que estuviera cumpliera mínimo con una de las dos ya sea entre 5 y 9 llamadas o 5 y 9 mensajes y bajo uso cuando se cumple una u otra. Por qué, porque el Alto Uso se estaba viendo afectado. Si le interesa a la empresa pudieramos hacer dos tablas separadas una por mensajes y otra por llamadas para ser más acertivos y que no afecte una u otra por la condición de cualquier de las dos.
Mi recomendación primero que nada sería hacer una análisis del uso de GB, ya que hoy en día con los jovenes (sector de edad menos importante para la compañia), es muy importante por todas las redes sociales, por lo que quizás, si hubiera algún incentivo como IG incluido o What's App o TikTok o una de las redes sociales del momento, quizás con este tipo de promociones pudieramos atraer más la atención de jovenes e incrementar la presencia de ese sector en la compañia. 
Por otro lado, lo que yo haría es reducir un poco el costo del plan Premium o por tiempo limitado poner una promoción para ver si la gente quiere subir de nivel su plan, sin embargo, como comento hay que hacer un análisis de los GB en uso, porque creo que hoy en día es un factor importante y las llamadas y mensajes cada vez pasan más a segundo plano. 
En resumidas cuentas, recomendaría seguir con esos dos planes, ya que crear un plan superior a premium creo que no es necesario ya que justo la demanda de los usuario dicta que incluso casi con el Básico es suficiente. Y agregar uno antes del Básico, sería como darnos un balazo en el pie porque quizás estaríamos migrando algunos Básicos a un plan con menor costo. Lo único que recomiendo es hacer alguna promoción en el Premium esperando algunos usuarios nuevos o actuales realicen el cambio y hacer un análisis sobre el uso de datos ya que quizás esa información si nos da más para poder crear alguna otra estratégia. 


Español :
### Atomicidad 
La atomicidad, son agrupaciones que funcionan como una misma entidad, normalmente se usan en transacciones .

Ejemplo de esto : Retirar un usuario de una base de datos, requiere que se agrupen un conjunto de entidades para que se asegure que el usuario este retirado de todos los lugares de la base de datos. 

### Consistencia 
La consistencia son un conjunto de reglas que definen los atributos de las tablas.
 
Ejemplo : Cuando uno crea cualquier tabla define el tipo de datos de los atributos, su longitud y sus llaves primarias. 

### Aislamiento (isolation) 
Las transacciones deben estar protegidas unas entre otras. 

Ejemplo: Cuando estamos editando un registro este no puede ser leído hasta que se termine de actualizar este anterior registro. 

### Durabilidad : 
La durabilidad, es que siempre que exista una transacción, siempre va a persistir en la base de datos. 

Ejemplo: En general es la base de sql. 




- 
1. Realiza una exportación del esquema de SCOTT usando la consola Enterprise Manager, con las siguientes condiciones:

* Exporta tanto la estructura de las tablas como los datos de las mismas.
* Excluye la tabla SALGRADE y los departamentos que no tienen empleados.
* Programa la operación para dentro de 15 minutos.
* Genera un archivo de log en el directorio raíz de ORACLE.

Realiza ahora la operación con Oracle Data Pump.

2. Importa el fichero obtenido anteriormente usando Enterprise Manager pero en un usuario distinto de otra base de datos.

3. Realiza una exportación de la estructura y los datos de todas las tablas de la base de datos usando el comando expdp de Oracle Data Pump encriptando la información. Prueba todas las posibles opciones que ofrece dicho comando y documentándolas adecuadamente.

4. Intenta realizar operaciones similares de importación y exportación con las herramientas proporcionadas con Postgres desde línea de comandos, documentando el proceso.

5. Exporta los documentos de una colección de MongoDB que cumplan una determinada condición e impórtalos en otra base de datos.
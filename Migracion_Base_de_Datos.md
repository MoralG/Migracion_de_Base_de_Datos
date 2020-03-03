
### Tarea 1
**Realiza una exportación del esquema de SCOTT usando la consola Enterprise Manager, con las siguientes condiciones:**

* Exporta tanto la estructura de las tablas como los datos de las mismas.
* Excluye la tabla SALGRADE y los departamentos que no tienen empleados.
* Programa la operación para dentro de 15 minutos.
* Genera un archivo de log en el directorio raíz de ORACLE.

**Realiza ahora la operación con Oracle Data Pump.**

* Exporta tanto la estructura de las tablas como los datos de las mismas.

Vamos a exportar el esquema SCOTT con el comando `expdp` indicandoles varios parametros pero antes vamos a indicarle la dirección donde queremos guardar el fichero `SchemaScott.dmp` y asignar privilegios necesarios.

Creamos el directorio donde guardaremos el fichero de importación:
~~~
mkdir /home/oracle/datapump
~~~

Desde el sql*Plus:
```sql
CREATE DIRECTORY dp AS '/home/oracle/datapump';
GRANT READ, WRITE ON DIRECTORY dp to system;
```

Los parametros que vamos a utilizar son:

> dumfile: Le indicamos el fichero de importación.
> 
> logfile: Le indicamos el nombre del fichero de log.
> 
> schema: Indicamos el esquema que queramos importar.
>
> directory: Le indicamos el nombre que le hemos asignado a la ruta.

~~~
expdp system/Oracle19 schemas=SCOTT directory=dp dumpfile=SchemaScott.dmp logfile=SchemaScott.log

    Export: Release 12.2.0.1.0 - Production on Mié Feb 19 20:53:43 2020

    Copyright (c) 1982, 2017, Oracle and/or its affiliates.  All rights reserved.

    Conectado a: Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production

    Advertencia: Las operaciones de Oracle Data Pump no se necesitan normalmente cuando se conecta a la     raíz o al elemento inicial de una base de datos del contenedor.

    Iniciando "SYSTEM"."SYS_EXPORT_SCHEMA_02":  system/******** schemas=SCOTT directory=dp  dumpfile=SchemaScott.dmp logfile=SchemaScott.log 
    Procesando el tipo de objeto SCHEMA_EXPORT/DEFAULT_ROLE
    Procesando el tipo de objeto SCHEMA_EXPORT/PRE_SCHEMA/PROCACT_SCHEMA
    La tabla maestra "SYSTEM"."SYS_EXPORT_SCHEMA_02" se ha cargado/descargado correctamente
    ******************************************************************************
    El juego de archivos de volcado para SYSTEM.SYS_EXPORT_SCHEMA_02 es:
      /home/oracle/datapump/SchemaScott.dmp
    El trabajo "SYSTEM"."SYS_EXPORT_SCHEMA_02" ha terminado correctamente en Mié Feb 19 20:54:03 2020   elapsed 0 00:00:19
~~~

Como podemos ver nos ha creado el fichero `SchemaScott.dmp` en la dirección `/home/oracle/datapump`.

* Excluye la tabla SALGRADE y los departamentos que no tienen empleados.


* Programa la operación para dentro de 15 minutos.



* Genera un archivo de log en el directorio raíz de ORACLE.

### Tarea 2
**Importa el fichero obtenido anteriormente usando Enterprise Manager pero en un usuario distinto de otra base de datos.**

Tenemos que utilizar el comando `impdp` y añadir un nuevo parametro, `remap_schema` donde le indicamos dos esquemas de usuario, primero el que queremos copiar y el segundo el esquema destino.

~~~
impdp system/Oracle19 dumpfile=SchemaScott.dmp directory=dp remap_schema=SCOTT:MORALG

  Import: Release 12.2.0.1.0 - Production on Jue Feb 20 10:26:12 2020

  Copyright (c) 1982, 2017, Oracle and/or its affiliates.  All rights reserved.

  Conectado a: Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production

  Advertencia: Las operaciones de Oracle Data Pump no se necesitan normalmente cuando se conecta a la   raíz o al elemento inicial de una base de datos del contenedor.

  La tabla maestra "SYSTEM"."SYS_IMPORT_FULL_01" se ha cargado/descargado correctamente
  Iniciando "SYSTEM"."SYS_IMPORT_FULL_01":  system/******** dumpfile=SchemaScott.dmp directory=dp   REMAP_SCHEMA=SCOTT:MORALG 
  Procesando el tipo de objeto SCHEMA_EXPORT/DEFAULT_ROLE
  Procesando el tipo de objeto SCHEMA_EXPORT/PRE_SCHEMA/PROCACT_SCHEMA
  El trabajo "SYSTEM"."SYS_IMPORT_FULL_01" ha terminado correctamente en Jue Feb 20 10:26:42 2020 elapsed   0 00:00:13
~~~
### Tarea3
**Realiza una exportación de la estructura y los datos de todas las tablas de la base de datos usando el comando expdp de Oracle Data Pump encriptando la información. Prueba todas las posibles opciones que ofrece dicho comando y documentándolas adecuadamente.**

Necesitamos que el usuario que va a realizar la exportación tenga los siguientes privilegios:
```sql
GRANT EXP_FULL_DATABASE to system;
GRANT IMP_FULL_DATABASE to system;
```
Luego tenemos que indicarle el parametro `full=Y`.

> **NOTA**: la opción `full=Y` indica que todos los datos y metadatos deben exportarse.

~~~
expdp system/Oracle19 full=Y directory=dp dumpfile=CopiaCompleta.dmp logfile=CopiaCompleta.log

  Export: Release 12.2.0.1.0 - Production on Jue Feb 20 09:47:56 2020

  Copyright (c) 1982, 2017, Oracle and/or its affiliates.  All rights reserved.

  Conectado a: Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production

  Advertencia: Las operaciones de Oracle Data Pump no se necesitan normalmente cuando se conecta a la   raíz o al elemento inicial de una base de datos del contenedor.

  Iniciando "SYSTEM"."SYS_EXPORT_FULL_01":  system/******** full=Y directory=dp dumpfile=CopiaCompleta. dmp logfile=CopiaCompleta.log 
  Procesando el tipo de objeto DATABASE_EXPORT/EARLY_OPTIONS/VIEWS_AS_TABLES/TABLE_DATA
  Procesando el tipo de objeto DATABASE_EXPORT/NORMAL_OPTIONS/TABLE_DATA
  Procesando el tipo de objeto DATABASE_EXPORT/NORMAL_OPTIONS/VIEWS_AS_TABLES/TABLE_DATA
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/TABLE_DATA
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/INDEX/STATISTICS/INDEX_STATISTICS
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/STATISTICS/TABLE_STATISTICS
  Procesando el tipo de objeto DATABASE_EXPORT/STATISTICS/MARKER
  Procesando el tipo de objeto DATABASE_EXPORT/PRE_SYSTEM_IMPCALLOUT/MARKER
  Procesando el tipo de objeto DATABASE_EXPORT/PRE_INSTANCE_IMPCALLOUT/MARKER
  Procesando el tipo de objeto DATABASE_EXPORT/TABLESPACE
  Procesando el tipo de objeto DATABASE_EXPORT/PROFILE
  Procesando el tipo de objeto DATABASE_EXPORT/SYS_USER/USER
  Procesando el tipo de objeto DATABASE_EXPORT/RADM_FPTM
  Procesando el tipo de objeto DATABASE_EXPORT/GRANT/SYSTEM_GRANT/PROC_SYSTEM_GRANT
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/GRANT/SYSTEM_GRANT
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/ROLE_GRANT
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/DEFAULT_ROLE
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/ON_USER_GRANT
  Procesando el tipo de objeto DATABASE_EXPORT/RESOURCE_COST
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/DB_LINK
  Procesando el tipo de objeto DATABASE_EXPORT/TRUSTED_DB_LINK
  Procesando el tipo de objeto DATABASE_EXPORT/DIRECTORY/DIRECTORY
  Procesando el tipo de objeto DATABASE_EXPORT/DIRECTORY/GRANT/OWNER_GRANT/OBJECT_GRANT
  Procesando el tipo de objeto DATABASE_EXPORT/SYSTEM_PROCOBJACT/PRE_SYSTEM_ACTIONS/PROCACT_SYSTEM
  Procesando el tipo de objeto DATABASE_EXPORT/SYSTEM_PROCOBJACT/PROCOBJ
  Procesando el tipo de objeto DATABASE_EXPORT/SYSTEM_PROCOBJACT/POST_SYSTEM_ACTIONS/PROCACT_SYSTEM
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/PROCACT_SCHEMA
  Procesando el tipo de objeto DATABASE_EXPORT/EARLY_OPTIONS/VIEWS_AS_TABLES/TABLE
  Procesando el tipo de objeto DATABASE_EXPORT/EARLY_POST_INSTANCE_IMPCALLOUT/MARKER
  Procesando el tipo de objeto DATABASE_EXPORT/NORMAL_OPTIONS/TABLE
  Procesando el tipo de objeto DATABASE_EXPORT/NORMAL_OPTIONS/VIEWS_AS_TABLES/TABLE
  Procesando el tipo de objeto DATABASE_EXPORT/NORMAL_POST_INSTANCE_IMPCALLOUT/MARKER
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/TABLE
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/COMMENT
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/INDEX/INDEX
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/TABLE/CONSTRAINT/CONSTRAINT
  Procesando el tipo de objeto DATABASE_EXPORT/FINAL_POST_INSTANCE_IMPCALLOUT/MARKER
  Procesando el tipo de objeto DATABASE_EXPORT/SCHEMA/POST_SCHEMA/PROCACT_SCHEMA
  Procesando el tipo de objeto DATABASE_EXPORT/AUDIT_UNIFIED/AUDIT_POLICY_ENABLE
  Procesando el tipo de objeto DATABASE_EXPORT/AUDIT
  Procesando el tipo de objeto DATABASE_EXPORT/POST_SYSTEM_IMPCALLOUT/MARKER
  . . "SYS"."KU$_USER_MAPPING_VIEW"               6.257 KB      47 filas exportadas
  . . "SYS"."AUD$"                                27.26 KB      24 filas exportadas
  . . "SYSTEM"."REDO_DB"                          25.59 KB       1 filas exportadas
  . . "ORDDATA"."ORDDCM_DOCS"                     252.9 KB       9 filas exportadas
  . . "WMSYS"."WM$WORKSPACES_TABLE$"              12.10 KB       1 filas exportadas
  . . "WMSYS"."WM$HINT_TABLE$"                    9.984 KB      97 filas exportadas
  . . "SYS"."DAM_CLEANUP_EVENTS$"                 7.320 KB       4 filas exportadas
  . . "LBACSYS"."OLS$INSTALLATIONS"               6.960 KB       2 filas exportadas
  . . "WMSYS"."WM$WORKSPACE_PRIV_TABLE$"          7.078 KB      11 filas exportadas
  . . "SYS"."DAM_CONFIG_PARAM$"                   6.531 KB      14 filas exportadas
  . . "SYS"."TSDP_SUBPOL$"                        6.328 KB       1 filas exportadas
  . . "WMSYS"."WM$NEXTVER_TABLE$"                 6.375 KB       1 filas exportadas
  . . "LBACSYS"."OLS$PROPS"                       6.234 KB       5 filas exportadas
  . . "WMSYS"."WM$ENV_VARS$"                      6.015 KB       3 filas exportadas
  . . "SYS"."TSDP_PARAMETER$"                     5.953 KB       1 filas exportadas
  . . "SYS"."TSDP_POLICY$"                        5.921 KB       1 filas exportadas
  . . "WMSYS"."WM$VERSION_HIERARCHY_TABLE$"       5.984 KB       1 filas exportadas
  . . "WMSYS"."WM$EVENTS_INFO$"                   5.812 KB      12 filas exportadas
  . . "LBACSYS"."OLS$AUDIT_ACTIONS"               5.757 KB       8 filas exportadas
  . . "LBACSYS"."OLS$DIP_EVENTS"                  5.539 KB       2 filas exportadas
  . . "LBACSYS"."OLS$AUDIT"                           0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$COMPARTMENTS"                    0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$DIP_DEBUG"                       0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$GROUPS"                          0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$LAB"                             0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$LEVELS"                          0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$POL"                             0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$POLICY_ADMIN"                    0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$POLS"                            0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$POLT"                            0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$PROFILE"                         0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$PROFILES"                        0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$PROG"                            0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$SESSINFO"                        0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$USER"                            0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$USER_COMPARTMENTS"               0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$USER_GROUPS"                     0 KB       0 filas exportadas
  . . "LBACSYS"."OLS$USER_LEVELS"                     0 KB       0 filas exportadas
  . . "SYS"."DAM_CLEANUP_JOBS$"                       0 KB       0 filas exportadas
  . . "SYS"."TSDP_ASSOCIATION$"                       0 KB       0 filas exportadas
  . . "SYS"."TSDP_CONDITION$"                         0 KB       0 filas exportadas
  . . "SYS"."TSDP_FEATURE_POLICY$"                    0 KB       0 filas exportadas
  . . "SYS"."TSDP_PROTECTION$"                        0 KB       0 filas exportadas
  . . "SYS"."TSDP_SENSITIVE_DATA$"                    0 KB       0 filas exportadas
  . . "SYS"."TSDP_SENSITIVE_TYPE$"                    0 KB       0 filas exportadas
  . . "SYS"."TSDP_SOURCE$"                            0 KB       0 filas exportadas
  . . "SYSTEM"."REDO_LOG"                             0 KB       0 filas exportadas
  . . "WMSYS"."WM$BATCH_COMPRESSIBLE_TABLES$"         0 KB       0 filas exportadas
  . . "WMSYS"."WM$CONS_COLUMNS$"                      0 KB       0 filas exportadas
  . . "WMSYS"."WM$CONSTRAINTS_TABLE$"                 0 KB       0 filas exportadas
  . . "WMSYS"."WM$LOCKROWS_INFO$"                     0 KB       0 filas exportadas
  . . "WMSYS"."WM$MODIFIED_TABLES$"                   0 KB       0 filas exportadas
  . . "WMSYS"."WM$MP_GRAPH_WORKSPACES_TABLE$"         0 KB       0 filas exportadas
  . . "WMSYS"."WM$MP_PARENT_WORKSPACES_TABLE$"        0 KB       0 filas exportadas
  . . "WMSYS"."WM$NESTED_COLUMNS_TABLE$"              0 KB       0 filas exportadas
  . . "WMSYS"."WM$RESOLVE_WORKSPACES_TABLE$"          0 KB       0 filas exportadas
  . . "WMSYS"."WM$RIC_LOCKING_TABLE$"                 0 KB       0 filas exportadas
  . . "WMSYS"."WM$RIC_TABLE$"                         0 KB       0 filas exportadas
  . . "WMSYS"."WM$RIC_TRIGGERS_TABLE$"                0 KB       0 filas exportadas
  . . "WMSYS"."WM$UDTRIG_DISPATCH_PROCS$"             0 KB       0 filas exportadas
  . . "WMSYS"."WM$UDTRIG_INFO$"                       0 KB       0 filas exportadas
  . . "WMSYS"."WM$VERSION_TABLE$"                     0 KB       0 filas exportadas
  . . "WMSYS"."WM$VT_ERRORS_TABLE$"                   0 KB       0 filas exportadas
  . . "WMSYS"."WM$WORKSPACE_SAVEPOINTS_TABLE$"        0 KB       0 filas exportadas
  . . "MDSYS"."RDF_PARAM$"                        6.515 KB       3 filas exportadas
  . . "SYS"."AUDTAB$TBS$FOR_EXPORT"               5.953 KB       2 filas exportadas
  . . "SYS"."DBA_SENSITIVE_DATA"                      0 KB       0 filas exportadas
  . . "SYS"."DBA_TSDP_POLICY_PROTECTION"              0 KB       0 filas exportadas
  . . "SYS"."FGA_LOG$FOR_EXPORT"                  18.19 KB       2 filas exportadas
  . . "SYS"."NACL$_ACE_EXP"                           0 KB       0 filas exportadas
  . . "SYS"."NACL$_HOST_EXP"                      6.976 KB       2 filas exportadas
  . . "SYS"."NACL$_WALLET_EXP"                        0 KB       0 filas exportadas
  . . "SYS"."SQL$_DATAPUMP"                           0 KB       0 filas exportadas
  . . "SYS"."SQLOBJ$AUXDATA_DATAPUMP"                 0 KB       0 filas exportadas
  . . "SYS"."SQLOBJ$DATA_DATAPUMP"                    0 KB       0 filas exportadas
  . . "SYS"."SQLOBJ$_DATAPUMP"                        0 KB       0 filas exportadas
  . . "SYS"."SQLOBJ$PLAN_DATAPUMP"                    0 KB       0 filas exportadas
  . . "SYS"."SQL$TEXT_DATAPUMP"                       0 KB       0 filas exportadas
  . . "SYSTEM"."SCHEDULER_JOB_ARGS"                   0 KB       0 filas exportadas
  . . "SYSTEM"."SCHEDULER_PROGRAM_ARGS"           9.515 KB      12 filas exportadas
  . . "WMSYS"."WM$EXP_MAP"                        7.718 KB       3 filas exportadas
  . . "WMSYS"."WM$METADATA_MAP"                       0 KB       0 filas exportadas
  . . "LUIS"."HOLA"                                   0 KB       0 filas exportadas
  . . "SYSTEM"."SYS_EXPORT_SCHEMA_01"                 0 KB       0 filas exportadas
  La tabla maestra "SYSTEM"."SYS_EXPORT_FULL_01" se ha cargado/descargado correctamente
  ******************************************************************************
  El juego de archivos de volcado para SYSTEM.SYS_EXPORT_FULL_01 es:
    /home/oracle/datapump/CopiaCompleta.dmp
  El trabajo "SYSTEM"."SYS_EXPORT_FULL_01" ha terminado correctamente en Jue Feb 20 09:51:44 2020 elapsed   0 00:03:41
~~~

Una vez terminado tendremos el fichero de la importación creado.
~~~
ls -lh datapump/CopiaCompleta.dmp 
  -rw-r----- 1 oracle oinstall 3,7M feb 20 09:51 datapump/CopiaCompleta.dmp
~~~

### Tarea 4
**Intenta realizar operaciones similares de importación y exportación con las herramientas proporcionadas con Postgres desde línea de comandos, documentando el proceso.**

En postgres se pueden importar y emportar las bases de datos de dos formas, con archivos **dump** o con archivos **sql**. Siempre es mas recomendable la egunda opción, ya que la primera puede dar problemas por la versión de PostgreSQL.

* Con archivos dump:

Importar base de datos
~~~
pg_dump -Fc -t nombre_tabla nombre_bbdd_origen -f /direccion_destino.dump
~~~

Exportar base de datos
~~~
pg_restore -t nombre_tabla -d nombre_bbdd_destino /direccion_destino.dump
~~~

* Con archivos sql:

Importar base de datos
~~~
pg_dump nombre_bbdd_origen -t nombre_tabla > /direccion_destino.sql
~~~

Exportar base de datos
~~~
psql -U username -W -h host nombre_bbdd_destino < direccion_destino.sql
~~~

Exportar base de datos (dentro del cliente pgsql)
~~~
\i /direccion_destino.sql
~~~

Vamos a probar si funciona:

~~~
moralgdb=# \d
                            List of relations
   Schema |               Name               |     Type      |  Owner   
  --------+----------------------------------+---------------+----------
   public | aspectos                         | foreign table | postgres
   public | conflictos                       | table         | moralg
   public | grupos_armados                   | table         | moralg
   public | historial_de_conflictos          | table         | moralg
   public | historial_intervenciones_armadas | table         | moralg
   public | paises                           | table         | moralg
  (6 rows)

moralgdb=# exit

postgres@Postgres:~$ ls
  11  audit.sql

postgres@Postgres:~$ pg_dump -Fc -t conflictos moralgdb -f /var/lib/postgresql/conflictos.dump

postgres@Postgres:~$ ls
  11  audit.sql  conflictos.dump

postgres@Postgres:~$ pg_restore -t conflictos -d paloma /var/lib/postgresql/conflictos.dump

postgres@Postgres:~$ psql
  psql (11.5 (Debian 11.5-1+deb10u1))
  Type help for help.

postgres=# \c paloma
  You are now connected to database "paloma" as user "postgres".

paloma=# \d
                   List of relations
   Schema |         Name          | Type  |  Owner   
  --------+-----------------------+-------+----------
   public | actividades           | table | postgres
   public | actividadesrealizadas | table | postgres
   public | conflictos            | table | moralg
   public | estancias             | table | postgres
   public | facturas              | table | postgres
   public | gastosextras          | table | postgres
   public | habitaciones          | table | postgres
   public | personas              | table | postgres
   public | regimenes             | table | postgres
   public | tarifas               | table | postgres
   public | temporadas            | table | postgres
   public | tiposdehabitacion     | table | postgres
  (12 rows)
~~~

### Tarea 5
**Exporta los documentos de una colección de MongoDB que cumplan una determinada condición e impórtalos en otra base de datos.**
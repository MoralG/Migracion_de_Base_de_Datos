# Grupal

## Tarea 1 
**Importad el fichero resultante de la exportación completa de las tablas y los datos de una instancia de ORACLE en otra instancia diferente empleando el comando impdp y explicad qué problemas surgen. Realizad un remapeo de esquemas si es necesario.**

~~~
ara realizar dicha tarea primeramente vamos a crear un usuario en el cual vamos a crear unas tablas, creamos un directorio para la exportación y posteriormente haremos la exportación.

- Creamos un tablespace para el usuario.



SQL> CREATE USER usuario1 IDENTIFIED BY usuario1 ;
SQL> CREATE USER usuario2 IDENTIFIED BY usuario2 ;
SQL> grant all privileges to usuario1;
SQL> grant all privileges to usuario2;

- Creamos las tablas.

create table solistas
(
IDinterprete_solista varchar2(10),
nombre_artistico varchar2(50),
instrumento varchar2(100),
constraint pk_solistas primary key (IDinterprete_solista),
constraint solistas_interpretes foreign key (IDinterprete_solista) references interpretes(id_interprete));



insert into solistas values ('124223','RUBEN FERNANDEZ','flauta');
insert into solistas values ('123456','MAURICE ANDRE','trompeta');
insert into solistas values ('1er456','CLAUDIO ARRAU','piano');
insert into solistas values ('654tgb','BARENBOIM','piano');
insert into solistas values ('543frtyh','YURI BASHMET','violin');
insert into solistas values ('12654f','HEINZ HOLLIGER','oboe');
insert into solistas values ('qazwsxedc','HOROWITZ','piano');
insert into solistas values ('1mkonji','MENUHIN','violin');
insert into solistas values ('1234567','PIERRE RAMPAL','flauta');
insert into solistas values ('123345','OISTRAKH','violin');


create table interpretes
(
id_interprete varchar2(10),
nombre_interprete varchar2(50),
constraint pk_interpretes primary key (id_interprete),
constraint contar check(length(id_interprete)>='5'));

insert into interpretes values ('123456','MAURICE ANDRE');
insert into interpretes values ('1er456','CLAUDIO ARRAU');
insert into interpretes values ('654tgb','DANIEL BARENBOIM');
insert into interpretes values ('543frtyh','YURI BASHMET');
insert into interpretes values ('12654f','HEINZ HOLLIGER');
insert into interpretes values ('qazwsxedc','VLADIMIR HOROWITZ');
insert into interpretes values ('1mkonji','YEHUDI MENUHIN');
insert into interpretes values ('1234567','MAURICE ANDRE');
insert into interpretes values ('123345','DAVID OISTRAKH');
insert into interpretes values ('123321','ITZHAK PERLMAN');
insert into interpretes values ('123565','MSTISLAV ROSTROPOVICH');
insert into interpretes values ('123422','Marc-André Hamelin ');
insert into interpretes values ('1234777','PIERRE RAMPAL');
insert into interpretes values ('124223','RUBEN FERNANDEZ');
insert into interpretes values ('12bgtyhn','Martha Argerich');


- Creamos el directorio de exportación.

SQL> create directory direc_expor as 'C:\app\alvaro\admin\orcl\';
-Damos permiso al usuario sobre el directorio.
SQL> GRANT READ, WRITE ON DIRECTORY direc_expor TO reco2;
SQL> GRANT READ, WRITE ON DIRECTORY direc_expor TO reco2;

- Realizamos primeramente la exportación .


C:\Windows\system32>expdp usuario1/usuario1 directory=direc_expor dumpfile=grupal11.dmp schemas=usuario1 logfile=grupalejer1.log

Export: Release 11.2.0.1.0 - Production on Dom Mar 1 17:46:28 2020

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.

Conectado a: Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit P
roduction
With the Partitioning, OLAP, Data Mining and Real Application Testing options
Iniciando "USUARIO1"."SYS_EXPORT_SCHEMA_01":  usuario1/******** directory=direc_
expor dumpfile=grupal1Ejer1.dmp schemas=usuario1 logfile=grupalejer1.log
Estimaci¾n en curso mediante el mÚtodo BLOCKS...
Procesando el tipo de objeto SCHEMA_EXPORT/TABLE/TABLE_DATA
Estimaci¾n total mediante el mÚtodo BLOCKS: 64 KB
Procesando el tipo de objeto SCHEMA_EXPORT/PRE_SCHEMA/PROCACT_SCHEMA
Procesando el tipo de objeto SCHEMA_EXPORT/TABLE/TABLE
Procesando el tipo de objeto SCHEMA_EXPORT/TABLE/INDEX/INDEX
Procesando el tipo de objeto SCHEMA_EXPORT/TABLE/CONSTRAINT/CONSTRAINT
Procesando el tipo de objeto SCHEMA_EXPORT/TABLE/INDEX/STATISTICS/INDEX_STATISTI
CS
Procesando el tipo de objeto SCHEMA_EXPORT/TABLE/COMMENT
. . "USUARIO1"."INTERPRETES"                    5.843 KB      15 filas exportada
s
La tabla maestra "USUARIO1"."SYS_EXPORT_SCHEMA_01" se ha cargado/descargado corr
ectamente
******************************************************************************
El juego de archivos de volcado para USUARIO1.SYS_EXPORT_SCHEMA_01 es:
  C:\APP\ALVARO\ADMIN\ORCL\GRUPAL1EJER1.DMP
El trabajo "USUARIO1"."SYS_EXPORT_SCHEMA_01" ha terminado correctamente en 17:47
:22


Importamos el fichero de exportación anteriormente creado en el usuario2 y comprobamos si se a realizado corrextamente.

C:\Users\alvaro>impdp usuario2/usuario2 dumpfile=grupal1Ejer1.dmp  DIRECTORY=dir
ec_expor logfile=impdp.log

Import: Release 11.2.0.1.0 - Production on Dom Mar 1 18:43:47 2020

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.

Conectado a: Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit P
roduction
With the Partitioning, OLAP, Data Mining and Real Application Testing options
ORA-39154: Los objetos de esquemas ajenos se han eliminado de la importaci¾n
La tabla maestra "USUARIO2"."SYS_IMPORT_FULL_01" se ha cargado/descargado correc
tamente
Iniciando "USUARIO2"."SYS_IMPORT_FULL_01":  usuario2/******** dumpfile=grupal1Ej
er1.dmp DIRECTORY=direc_expor logfile=impdp.log
El trabajo "USUARIO2"."SYS_IMPORT_FULL_01" ha terminado correctamente en 18:43:5
2

- Comprobamos que se ha realizado la importación.

SQL> insert into interpretes values ('124223','RUBEN FERNANDEZ');

1 fila creada.

SQL> insert into interpretes values ('12bgtyhn','Martha Argerich');

1 fila creada.

SQL> select * from interpretes;

ID_INTERPR NOMBRE_INTERPRETE
---------- --------------------------------------------------
123456     MAURICE ANDRE
1er456     CLAUDIO ARRAU
654tgb     DANIEL BARENBOIM
543frtyh   YURI BASHMET
12654f     HEINZ HOLLIGER
qazwsxedc  VLADIMIR HOROWITZ
1mkonji    YEHUDI MENUHIN
1234567    MAURICE ANDRE
123345     DAVID OISTRAKH
123321     ITZHAK PERLMAN
123565     MSTISLAV ROSTROPOVICH

ID_INTERPR NOMBRE_INTERPRETE
---------- --------------------------------------------------
123422     Marc-André Hamelin
1234777    PIERRE RAMPAL
124223     RUBEN FERNANDEZ
12bgtyhn   Martha Argerich

15 filas seleccionadas.

SQL>
~~~

## Tarea 2
**Cread la estructura de tablas de uno de vuestros proyectos de 1º en ORACLE y, mediante una exportación cread un script básico de creación de las tablas con las respectivas restricciones. Realizad de la forma más automatizada posible las acciones necesarias para transformar ese script generado por ORACLE en un script de creación de tablas para Postgres. Documentar todas las acciones realizadas y el código usado para llevarlas a cabo.**

Creamos los procedimientos necesarios.
```sql
CREATE OR REPLACE PROCEDURE OracleToPostgres(p_Owner VARCHAR2)
IS
    cursor c_tablas is
    select table_name
    from dba_tables
    where owner=p_Owner;
BEGIN
    for v_tablas in c_tablas loop
        dbms_output.put_line(chr(9));
        dbms_output.put_line('CREATE TABLE '||v_tablas.table_name||' (');
        SacarFilasTabla(v_tablas.table_name);
        SaberConstraintsTabla(v_tablas.table_name);
        dbms_output.put_line(');');
    end loop;
END;
/


CREATE OR REPLACE PROCEDURE SacarFilasTabla(p_nombretabla VARCHAR2)
IS
    cursor c_indices is
    select column_name,data_type,data_length
    from dba_tab_columns
    where table_name=p_nombretabla;
    v_tipo VARCHAR2(40);
BEGIN
    for v_indices in c_indices loop
        case v_indices.data_type
            when 'VARCHAR2' then
                v_tipo:='VARCHAR';
            when 'NUMBER' then
                v_tipo:='NUMERIC';
            when 'DATE' then
                v_tipo:='TIMESTAMP';
        end case;
        dbms_output.put_line(chr(9)||RPAD(v_indices.column_name,25)||v_tipo||'('||v_indices.data_length||')'||',');
    end loop;
END;
/


CREATE OR REPLACE FUNCTION Sacar_PK_FK_Unique(p_constraint VARCHAR2,
                                              p_owner VARCHAR2)
RETURN VARCHAR2
IS
    cursor c_primaryUnique is
    select COLUMN_NAME
    from DBA_CONS_COLUMNS
    where CONSTRAINT_NAME=p_constraint
    and owner=p_owner;

    v_cadena VARCHAR2(100);
    v_contador NUMBER(2):=0;
BEGIN
    for v_primaryUnique in c_primaryUnique loop
        if v_contador = 0 then
            v_cadena:=v_primaryUnique.column_name;
        else
            v_cadena:=v_cadena||', '||v_primaryUnique.column_name;
        end if;
        v_contador:=v_contador+1;
    end loop;
    return v_cadena;
END;
/


CREATE OR REPLACE FUNCTION SacarReferencias(p_r_constraint VARCHAR2,
                                            p_owner VARCHAR2)
RETURN VARCHAR2
IS
    v_cadena VARCHAR2(100);
BEGIN
    select table_name into v_cadena
    from dba_constraints
    where constraint_name=p_r_constraint
    and owner=p_owner;
    return v_cadena;
END;
/


Create or replace function CambiarARegexpPostgres(p_constraint VARCHAR2)
return VARCHAR2
is
	v_postgres VARCHAR2(500);
BEGIN
	select replace(p_constraint,'REGEXP_LIKE(','') into v_postgres
	from dual;
	select REGEXP_REPLACE(v_postgres,',',' ~',1,1) into v_postgres
	from dual;
    select rtrim(v_postgres,')') into v_postgres
	from dual;
	return v_postgres;
END;
/


CREATE OR REPLACE PROCEDURE SaberConstraintsTabla(p_nombretabla VARCHAR2)
IS
    cursor c_constraint is
    select constraint_name,constraint_type,search_condition,R_CONSTRAINT_NAME,OWNER
    from dba_constraints
    where table_name=p_nombretabla;
    cont_Comas NUMBER:=0;
BEGIN
    for v_constraint in c_constraint loop
        if cont_Comas >= 1 then
            dbms_output.put_line(',');
        end if;
        case v_constraint.constraint_type
            when 'C' then
                MostrarConstraintCheck(v_constraint.constraint_name, v_constraint.search_condition);
            when 'P' then
                MostrarConstraintPrimaryKey(v_constraint.constraint_name, v_constraint.owner);
            when 'R' Then
                MostrarConstraintForeignKey(v_constraint.constraint_name, v_constraint.r_constraint_name, v_constraint.owner);
            when 'U' then
                MostrarConstraintUniqueKey(v_constraint.constraint_name, v_constraint.owner);
        end case;
        cont_Comas:=cont_Comas+1;
    end loop;
END;
/


CREATE OR REPLACE PROCEDURE MostrarConstraintCheck(p_ConstraintName VARCHAR2,
                                                   p_TextCheck VARCHAR2)
IS
    v_tipo VARCHAR2(500);
    v_regexp VARCHAR2(500);
BEGIN
    if p_TextCheck like '%regexp_like%' or p_TextCheck like '%REGEXP_LIKE%' then
        v_regexp:=CambiarARegexpPostgres(p_TextCheck);
        v_tipo:=' check('||v_regexp||')';
    else
        v_tipo:=' check('||p_TextCheck||')';
    end if;
    dbms_output.put_line(chr(9)||'CONSTRAINT '||p_ConstraintName||v_tipo);
END;
/


CREATE OR REPLACE PROCEDURE MostrarConstraintPrimaryKey(p_ConstraintName VARCHAR2,
                                                        p_Owner VARCHAR2)
IS
    v_tipo VARCHAR2(500);
    v_primary VARCHAR2(100);
BEGIN
    v_primary:=Sacar_PK_FK_Unique(p_ConstraintName, p_Owner);
    v_tipo:=' PRIMARY KEY('||v_primary||')';
    dbms_output.put_line(chr(9)||'CONSTRAINT '||p_ConstraintName||v_tipo);
END;
/


CREATE OR REPLACE PROCEDURE MostrarConstraintForeignKey(p_ConstraintName VARCHAR2,
                                                        p_ReferenceConstraint VARCHAR2,
                                                        p_Owner VARCHAR2)
IS
    v_tipo VARCHAR2(500);
    v_foreignkey VARCHAR2(100);
    v_references VARCHAR2(100);
    v_primaryreferences VARCHAR2(100);
BEGIN
    v_foreignkey:=Sacar_PK_FK_Unique(p_ConstraintName, p_Owner);
    v_references:=SacarReferencias(p_ReferenceConstraint, p_Owner);
    v_primaryreferences:=Sacar_PK_FK_Unique(p_ReferenceConstraint, p_Owner);
    v_tipo:=' FOREIGN KEY('||v_foreignkey||') REFERENCES '||v_references||'('||v_primaryreferences||')';
    dbms_output.put_line(chr(9)||'CONSTRAINT '||p_ConstraintName||v_tipo);
END;
/


CREATE OR REPLACE PROCEDURE MostrarConstraintUniqueKey(p_ConstraintName VARCHAR2,
                                                       p_Owner VARCHAR2)
IS
    v_tipo VARCHAR2(500);
    v_unique VARCHAR2(100);
BEGIN
    v_unique:=Sacar_PK_FK_Unique(p_ConstraintName, p_Owner);
    v_tipo:=' UNIQUE('||v_unique||')';
    dbms_output.put_line(chr(9)||'CONSTRAINT '||p_ConstraintName||v_tipo);
END;
/
```

Comprobamos la salida del procedimientos. Tenemos que tener activado `SET SERVEROUTPUT ON`.
```sql
exec OracleToPostgres('MORALG')
	
CREATE TABLE CONFLICTOS (
	CODIGO			 VARCHAR(3),
	NOMBRE			 VARCHAR(50),
	CAUSA			 VARCHAR(15),
	CONSTRAINT CAUSA_SELECCION check(upper(causa) in ('RACIAL','ECONOMICO','RELIGIOSO','DESCONOCIDA'))
,
	CONSTRAINT NOTNULL_NOMBRECONFL check(nombre IS NOT NULL)
,
	CONSTRAINT PK_CODIGO_CONFLICTOS PRIMARY KEY(CODIGO)
,
	CONSTRAINT UNIQUE_NOMBRECONFL UNIQUE(NOMBRE)
);
	
CREATE TABLE PAISES (
	CODIGO			 VARCHAR(5),
	NOMBRE			 VARCHAR(30),
	CAPITAL 		 VARCHAR(30),
	CONSTRAINT COD_LETRAPAIS_DOSDIG check(codigo ~ '^[A-Z]{2}[0-9]{2}$') AND SUBSTR(codigo,1,2) in ('EU','AS','AF','NA','SA','OC','AN')
,
	CONSTRAINT NOTNULL_NOMBREPAIS check(nombre IS NOT NULL)
,
	CONSTRAINT PK_CODIGO_PAISES PRIMARY KEY(CODIGO)
,
	CONSTRAINT UNIQUE_NOMBREPAIS UNIQUE(NOMBRE)
);
	
CREATE TABLE HISTORIAL_DE_CONFLICTOS (
	CODIGO_CONFLICTO	 VARCHAR(3),
	CODIGO_PAIS		 VARCHAR(5),
	NUM_HERIDOS		 NUMERIC(22),
	NUM_MUERTOS		 NUMERIC(22),
	CONSTRAINT PK_CODCONFLICTO_CODPAIS PRIMARY KEY(CODIGO_CONFLICTO, CODIGO_PAIS)
,
	CONSTRAINT FK_COD_CONFLICTO FOREIGN KEY(CODIGO_CONFLICTO) REFERENCES CONFLICTOS(CODIGO)
,
	CONSTRAINT FK_COD_PAIS FOREIGN KEY(CODIGO_PAIS) REFERENCES PAISES(CODIGO)
);
	
CREATE TABLE GRUPOS_ARMADOS (
	CODIGO			 VARCHAR(5),
	NOMBRE			 VARCHAR(50),
	CONSTRAINT CODIGO_ABC check(codigo ~ '^[ABC]-[0-9]{1,3}$')
,
	CONSTRAINT NOTNULL_NOMBREGRUP check(nombre IS NOT NULL)
,
	CONSTRAINT PK_CODIGO_GRUPARMADOS PRIMARY KEY(CODIGO)
,
	CONSTRAINT UNIQUE_NOMBREGRUP UNIQUE(NOMBRE)
);
	
CREATE TABLE HIS_INTERVENCIONES_ARMADAS (
	CODIGO_GRUPARMADO	 VARCHAR(5),
	CODIGO_CONFLICTO	 VARCHAR(3),
	FECHA_INCORPORACION	 TIMESTAMP(7),
	FECHA_RETIRADA		 TIMESTAMP(7),
	CONSTRAINT MAS_ABRIL2001 check(to_char(fecha_incorporacion, 'YYYY/MM') > '2001/04')
,
	CONSTRAINT PK_CODGRUPARM_CODCONF_FECHA PRIMARY KEY(CODIGO_GRUPARMADO, CODIGO_CONFLICTO, FECHA_INCORPORACION)
,
	CONSTRAINT FK_COD_GRUPARMADO_ARMADAS FOREIGN KEY(CODIGO_GRUPARMADO) REFERENCES GRUPOS_ARMADOS(CODIGO)
,
	CONSTRAINT FK_COD_CONFLICTO_ARMADAS FOREIGN KEY(CODIGO_CONFLICTO) REFERENCES CONFLICTOS(CODIGO)
);
	
CREATE TABLE ORGANIZACIONES (
	CODIGO			 VARCHAR(3),
	CODIGO_ORGDEPEN 	 VARCHAR(3),
	NOMBRE			 VARCHAR(30),
	NUMPERSONAS_CONFLICTO	 NUMERIC(22),
	TIPO			 VARCHAR(30),
	CONSTRAINT INICIALES_MAYUS check(nombre = INITCAP(nombre))
,
	CONSTRAINT NOTNULL_NOMBREORG check(nombre IS NOT NULL)
,
	CONSTRAINT NOTNULL_TIPOORG check(tipo IS NOT NULL)
,
	CONSTRAINT PK_CODIGO_ORGANIZACIONES PRIMARY KEY(CODIGO)
,
	CONSTRAINT UNIQUE_NOMBREORG UNIQUE(NOMBRE)
,
	CONSTRAINT FK_CODIGO_ORGDEPEND FOREIGN KEY(CODIGO_ORGDEPEN) REFERENCES ORGANIZACIONES(CODIGO)
);
	
CREATE TABLE HIS_INTERVENCIONES_MEDIADORAS (
	CODIGO_ORG		 VARCHAR(3),
	CODIGO_CONFLICTO	 VARCHAR(3),
	FECHA_INCORPORACION	 TIMESTAMP(7),
	FECHA_RETIRADA		 TIMESTAMP(7),
	TIPO_AYUDA		 VARCHAR(30),
	CONSTRAINT PK_ORGA_CONFLITOS_FECHA PRIMARY KEY(CODIGO_ORG, CODIGO_CONFLICTO, FECHA_INCORPORACION)
,
	CONSTRAINT FK_COD_ORGA FOREIGN KEY(CODIGO_ORG) REFERENCES ORGANIZACIONES(CODIGO)
,
	CONSTRAINT FK_COD_CONFLICTOS FOREIGN KEY(CODIGO_CONFLICTO) REFERENCES CONFLICTOS(CODIGO)
);
	
CREATE TABLE ENVIOS (
	CODIGO			 VARCHAR(3),
	CODIGO_ORG		 VARCHAR(3),
	FECHA_HORA		 TIMESTAMP(7),
	CONSTRAINT ENTRE8_16 check(to_char(fecha_hora, 'HH24:MI') between '08:00' and '16:00')
,
	CONSTRAINT DIA1_15 check(to_char(fecha_hora, 'DD') = '01' OR to_char(fecha_hora, 'DD') = '15')
,
	CONSTRAINT PK_CODIGO_ENVIOS PRIMARY KEY(CODIGO)
,
	CONSTRAINT FK_COD_ORG FOREIGN KEY(CODIGO_ORG) REFERENCES ORGANIZACIONES(CODIGO)
);
	
CREATE TABLE PRODUCTOS (
	NOMBRE			 VARCHAR(20),
	DESCRIPCION		 VARCHAR(100),
	CONSTRAINT PK_NOMBRE_PRODUCTOS PRIMARY KEY(NOMBRE)
);
	
CREATE TABLE CAMPOS_REFUGIADOS (
	CODIGO			 VARCHAR(3),
	NOMBRE			 VARCHAR(30),
	ZONA_ASENTAMIENTO	 VARCHAR(50),
	CONSTRAINT NOTNULL_NOMBRECAMP check(nombre IS NOT NULL)
,
	CONSTRAINT NOTNULL_ZONACAMP check(zona_asentamiento IS NOT NULL)
,
	CONSTRAINT PK_CODIGO_CAMPREFUGIADOS PRIMARY KEY(CODIGO)
,
	CONSTRAINT UNIQUE_NOMBRCAMP UNIQUE(NOMBRE)
);
	
CREATE TABLE PAQUETES (
	CODIGO			 VARCHAR(3),
	CODIGO_REFUGIO		 VARCHAR(3),
	CODIGO_ENVIO		 VARCHAR(3),
	NOMBRE_PRODUCTO 	 VARCHAR(30),
	CANTIDAD		 NUMERIC(22),
	CONSTRAINT NOTNULL_CANTIDADPAQUETE check(cantidad IS NOT NULL)
,
	CONSTRAINT PK_CODIGO_PAQUETES PRIMARY KEY(CODIGO)
,
	CONSTRAINT FK_COD_REFUGIADOS FOREIGN KEY(CODIGO_REFUGIO) REFERENCES CAMPOS_REFUGIADOS(CODIGO)
,
	CONSTRAINT FK_COD_ENVIO FOREIGN KEY(CODIGO_ENVIO) REFERENCES ENVIOS(CODIGO)
,
	CONSTRAINT FK_NOMBRE_PRODUCTOS FOREIGN KEY(NOMBRE_PRODUCTO) REFERENCES PRODUCTOS(NOMBRE)
);
	
CREATE TABLE REFUGIADOS (
	CODIGO			 VARCHAR(4),
	CODIGO_REFUGIO		 VARCHAR(4),
	NOMBRE			 VARCHAR(40),
	EDAD			 NUMERIC(22),
	FECHA_LLEGADA		 TIMESTAMP(7),
	FECHA_SALIDA		 TIMESTAMP(7),
	CONSTRAINT NOTNULL_NOMBREREFUG check(nombre IS NOT NULL)
,
	CONSTRAINT NOTNULL_EDADREFUG check(edad IS NOT NULL)
,
	CONSTRAINT PK_REFUGIADO_REFUGIO PRIMARY KEY(CODIGO, CODIGO_REFUGIO)
);

Procedimiento PL/SQL terminado correctamente.
```

## Tarea 3
**SQL\*Loader es una herramienta que sirve para cargar grandes volúmenes de datos en una instancia de ORACLE. Exportad los datos de uno de vuestros proyectos de 1º desde Postgres a  texto plano con delimitadores y emplead SQLLoader para realizar el proceso de carga de dichos datos a una instancia ORACLE. Debéis explicar los distintos ficheros de configuración y de log que tiene SQL\*Loader.**

Contamos con las siguientes tablas en nuestra base de datos Postgres:

```sql
invitado1_db=> \d
                    List of relations
 Schema |            Name            | Type  |   Owner   
--------+----------------------------+-------+-----------
 public | alumnos                    | table | invitado1
 public | asignaturas                | table | invitado1
 public | convocatorias              | table | invitado1
 public | docencias                  | table | invitado1
 public | matriculaciones            | table | invitado1
 public | personal                   | table | invitado1
 public | profesores                 | table | invitado1
 public | proyectos_de_investigacion | table | invitado1
 public | subvenciones               | table | invitado1
 public | titulaciones               | table | invitado1
(10 rows)
```

Vamos a realizar un script en bash llamado `scriptexportacion.sh` para facilitar la tarea:

```bash
#! \bin\bash

while :
do
echo ""
echo "~~~~~ EXPORTACIÓN ~~~~~~"
echo ""
echo "1. Realizar exportación"
echo "0. Salir"
read -p "Elija opción: " opcion

case $opcion in

1)

echo ""
echo "Introduzca el nombre de la base de datos a la que desea conectarse: "
read bd
echo "Introduzca el nombre de usuario: "
read user
echo "Introduzca la contraseña: "
read pwd
echo "Introduzca el nombre de la tabla: "
read tabla
echo ""

PGPASSWORD=$pwd psql -U $user -d $bd -c "COPY $tabla TO '/var/lib/postgresql/$tabla.txt' WITH (DELIMITER E'\t')"

echo ""
echo "Exportación realizada con exito"
echo "";;

0) exit;;

*) echo "Error de opción";;

esac
done
```

Vamos a realizar una prueba con una de nuestras tablas:

~~~
sh scriptexportacion.sh 

    ~~~~~ EXPORTACIÓN ~~~~~~

    1. Realizar exportación
    2. Salir
    Elija opción: 1

    Introduzca el nombre de la base de datos a la que desea conectarse: 
    invitado1_db
    Introduzca el nombre de usuario: 
    postgres
    Introduzca la contraseña: 
    dios
    Introduzca el nombre de la tabla: 
    alumnos

    COPY 4

    Exportación realizada con exito


    ~~~~~ EXPORTACIÓN ~~~~~~

    1. Realizar exportación
    0. Salir
    Elija opción: 0
~~~

Comprobamos que tenemos el archivo:

~~~
postgres@psqlserver:~$ cat alumnos.txt 
81753472H	Fátima	Simón	1991-03-01	Calle Real, 42
76837941R	Basilio	Elba	1992-09-10	Avenida de España, 31
16597980F	Brunilda	Telmo	1991-05-11	Calle Romera, 4
47585299R	Elías	Josepe	1998-08-05	Calle Bosque, 15
~~~

Ahora vamos a copiar en nuestra máquina Oracle el archivo exportado de PostgreSQL.

Tenemos que tener la misma estructura de las tablas, en Oracle, que hemos exportado en Postgres.
~~~
create table Alumnos
(
    DNI VARCHAR2(9),
    Nombre VARCHAR2(20),
    Apellidos VARCHAR2(20),
    FechaNacimiento DATE,
    Direccion VARCHAR2(50),
    constraint pk_alumnos PRIMARY KEY(DNI)
);
~~~

Tras esto, vamos a crear un fichero .ctl llamado `ficherocarga.ctl`, para que SQL Loader sea capaz de cargar los datos que exportamos de PostgreSQL:

~~~
LOAD DATA
INFILE '/home/oracle/alumnos.txt'
INTO TABLE alumnos
FIELDS TERMINATED BY x'09'
(
    DNI,
    NOMBRE,
    APELLIDOS,
    FECHANACIMIENTO,
    DIRECCION
)
~~~

Ahora vamos a cargar los datos en Oracle:

~~~
sqlldr system/RAUL control=ficherocarga.ctl data=alumnos.txt log=log.txt

    SQL*Loader: Release 12.1.0.2.0 - Production on Sáb Feb 29 20:19:24 2020

    Copyright (c) 1982, 2014, Oracle and/or its affiliates.  All rights reserved.

    Ruta de acceso utilizada:      Convencional
    Punto de confirmación alcanzado - recuento de registros lógicos 4

    Tabla ALUMNOS:
      4 Filas se ha cargado correctamente.

    Consulte el archivo log:
      log.txt
    para obtener más información sobre la carga.
~~~


Vamos a consultar también el log:

~~~
cat log.txt 

    SQL*Loader: Release 12.1.0.2.0 - Production on Sáb Feb 29 20:19:24 2020

    Copyright (c) 1982, 2014, Oracle and/or its affiliates.  All rights reserved.

    Archivo de Control:   ficherocarga.ctl
    Archivo de Datos:      alumnos.txt
      Archivo de Errores:     alumnos.bad
      Desechar Archivo:  ninguno especificado
    
     (Permitir todos los registros desechados)

    Número a cargar: ALL
    Número que omitir: 0
    Errores permitidos: 50
    Matriz de enlace:     64 filas, máximo de 256000 bytes
    Continuación:    ninguno especificado
    Ruta de acceso utilizada:      Convencional

    Tabla ALUMNOS, cargada de cada registro lógico.
    Opción INSERT activa para esta tabla: INSERT

       Nombre Columna                  Posición   Long  Term Entorno Tipo de Dato
    ------------------------------ ---------- ----- ---- ---- ---------------------
    DNI                                 FIRST     *  WHT      CHARACTER            
    NOMBRE                               NEXT     *  WHT      CHARACTER            
    APELLIDOS                            NEXT     *  WHT      CHARACTER            
    FECHANACIMIENTO                      NEXT     *  WHT      CHARACTER            
    DIRECCION                            NEXT     *  WHT      CHARACTER            


    Tabla ALUMNOS:
      4 Filas se ha cargado correctamente.
      0 Filas no cargada debido a errores de datos.
      0 Filas no cargada porque todas las cláusulas WHEN han fallado.
      0 Filas no cargada porque todos los campos eran nulos.


    Espacio asignado a matriz de enlace:            82560 bytes (64 filas)
    Bytes de buffer de lectura: 1048576

    Total de registros lógicos ignorados:          0
    Total de registros lógicos leídos:           4
    Total de registros lógicos rechazados:         0
    Total de registros lógicos desechados:        0

    La ejecución empezó en Sáb Feb 29 20:19:24 2020
    La ejecución terminó en Sáb Feb 29 20:19:24 2020

    Tiempo transcurrido:     00:00:00.07
    Tiempo de CPU:         00:00:00.03
~~~

Por último, realizamos un `select` para comprobar que los datos se han cargado correctamente:

```sql
SQL> select * from alumnos;

    DNI	  NOMBRE	       APELLIDOS	    FECHANAC
    --------- -------------------- -------------------- --------
    DIRECCION
    --------------------------------------------------
    81753472H Fátima               Simón                01/03/91
    Calle Real, 42

    76837941R Basilio	       Elba		    10/09/92
    Avenida de España, 31

    16597980F Brunilda	       Telmo		    11/05/91
    Calle Romera, 4


    DNI	  NOMBRE	       APELLIDOS	    FECHANAC
    --------- -------------------- -------------------- --------
    DIRECCION
    --------------------------------------------------
    47585299R Elías                Josepe               05/08/98
    Calle Bosque, 15
```
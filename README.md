# oracledb-omnidb
a docker compose for oracledb with omnidb

user is SYSTEM in oracle db

omnidb server=oracledb port=1521 user=SYSTEM database=ORCLCDB 
password=oracle123


enter in conitainer oracledb

```
docker exec -ti oracledb bash
```

Create a dir for plugglable database
```
mkdir -p /opt/oracle/oradata/ORCLCDB/springdb
```

Create the pluggable database with user and grant roles with sql plus inside container
```
sqlplus / as sysdba

CREATE PLUGGABLE DATABASE springdb ADMIN USER springadmin IDENTIFIED BY spring123 FILE_NAME_CONVERT = ('/opt/oracle/oradata/ORCLCDB/pdbseed','/opt/oracle/oradata/ORCLCDB/springdb');
```

Open database for external connections

```
ALTER SESSION SET CONTAINER = springdb;

alter pluggable database springdb OPEN;
alter database open;
```

Grant permissions to user
```
GRANT CONNECT, RESOURCE, DBA to springadmin;
```

Now you can connect to springdb with user springadmin pass spring123

Liquibase:
Inside springdb sql
```
CREATE TABLESPACE APP_DATA DATAFILE 'APP_DATA.dbf' SIZE 1m;
CREATE ROLE ROLE_DB_MASTER;
CREATE ROLE ROLE_DB_SLAVE;
CREATE SLAVE APPENGINE_MASTER IDENTIFIED BY spring123 DEFAULT TABLESPACE APP_DATA QUOTA UNLIMITED ON APP_DATA;
CREATE SLAVE APPENGINE_SLAVE IDENTIFIED BY spring123 DEFAULT TABLESPACE APP_DATA ;
GRANT CREATE PROCEDURE, CREATE SESSION, GLOBAL QUERY REWRITE, CREATE TRIGGER, DROP PUBLIC SYNONYM, CREATE SEQUENCE, CREATE MATERIALIZED VIEW, CREATE TYPE, CREATE VIEW, CREATE PUBLIC SYNONYM, CREATE TABLE to ROLE_DB_MASTER;
GRANT CREATE SESSION to ROLE_DB_SLAVE;
GRANT ROLE_DB_MASTER to APPENGINE_MASTER;
GRANT ROLE_DB_SLAVE to APPENGINE_SLAVE;
```
If blockin database not open after restart contianer
```
select NAME from v$datafile where file#=16;
alter database datafile 16 OFFLINE DROP;
alter database open;
```

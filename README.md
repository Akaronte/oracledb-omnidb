# oracledb-omnidb
a docker compose for oracledb with omnidb

user is SYSTEM in oracle db

omnidb server=oracledb port=1521 user=SYSTEM database=ORCLCDB 
password=oracle123


enter in conitainer oracledb


mkdir -p /opt/oracle/oradata/ORCLCDB/springdb


sqlplus / as sysdba

alter pluggable database springdb OPEN;

CREATE PLUGGABLE DATABASE springdb ADMIN USER springadmin IDENTIFIED BY spring123 FILE_NAME_CONVERT = ('/opt/oracle/oradata/ORCLCDB/pdbseed','/opt/oracle/oradata/ORCLCDB/springdb');

alter pluggable database springdb OPEN;


ALTER SESSION SET CONTAINER = springdb;
alter database open;

GRANT CONNECT, RESOURCE, DBA to springuser;



---------

SELECT USERNAME FROM ALL_USERS ORDER BY USERNAME; 
SELECT TABLESPACE_NAME FROM USER_TABLESPACES;


SELECT PDB_ID, PDB_NAME, STATUS FROM DBA_PDBS ORDER BY PDB_ID;

CREATE TABLESPACE ESDEFAULTSENGINE_DATA DATAFILE size 20G autoextend on next 1G;


CREATE TABLESPACE ESDEFAULTSENGINE_DATA;


SELECT TABLESPACE_NAME, STATUS, CONTENTS 2 FROM USER_TABLESPACES;

SELECT PDB_ID, PDB_NAME, STATUS FROM DBA_PDBS ORDER BY PDB_ID;



CREATE TABLESPACE MYAPP_DATA  DATAFILE 'myapp_data.dbf'  SIZE 1m;

SELECT 
   tablespace_name, 
   file_name, 
   bytes / 1024/ 1024  MB
FROM
   dba_data_files;
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
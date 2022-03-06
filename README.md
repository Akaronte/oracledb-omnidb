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

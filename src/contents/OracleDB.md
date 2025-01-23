---
author: Kuldeep
datetime: 2024-10-07
title: Oracle database
slug: "oracle-db"
featured: false
draft: false
tags:
  - oracle
  - database
ogImage: ""
description: Oracle database installation, management
---

# Install using Docker

## Run from compose file(preferred)

```bash
version: '3.8'

services:
  etns_db:
    image: oracledb19c/oracle.19.3.0-ee:oracle19.3.0-ee
    container_name: ETNS_db
    ports:
      - "1521:1521"
	stop_grace_period: 10m
    environment:
      ORACLE_SID: etnsdatabase
      ORACLE_PDB: ORCLPDB1
      ORACLE_PWD: ORCLCDB
      ORACLE_EDITION: enterprise
    volumes:
      - oracle_data:/opt/oracle/oradata
      - ~/dump_dir:/home/oracle/dump

volumes:
  oracle_data:
```

_Note_ Do not add network in compose. Manually create the network and connect to the container using

```bash
docker network create networkName
docker network connect networkName containerName
```

## Run as Docker run

```bash
docker run -d \
  --name ETNSCMS_db \
  -p 1521:1521 \
  -p 5500:5500 \
  -e ORACLE_SID=oracledatabase \
  -e ORACLE_PDB=ORCLPDB1 \
  -e ORACLE_PWD=ORCLCDB \
  -e ORACLE_EDITION=enterprise \
  -v oracle_data:/opt/oracle/oradata \
  -v /home/user-name/oracle/dump_dir:/home/oracle/dump \
  --network spring-oracle-network \
  --rm \
  oracledb19c/oracle.19.3.0-ee:oracle19.3.0-ee
```

## Management

```bash
# connect to database using this
sqlplus system/ORCLCDB@//localhost:1521/etnsdatabase

# do not change
CREATE DIRECTORY dump_dir AS '/home/oracle/dump';

# create full backup
expdp system/ORCLCDB@etnsdatabase FULL=Y DUMPFILE=full_db_backup.dmp DIRECTORY=dump_dir LOGFILE=expdp_full_db.log

# make sure the dump and log files have oracle oinstall user and group
impdp system/ORCLCDB@etnsdatabase FULL=Y DUMPFILE=full_db_backup.dmp DIRECTORY=dump_dir LOGFILE=impdp_full_db.log
```

_Note_ here etnsdatabase is treated as service name in docker

_Note_ system/ORCLCDB@//localhost:1521/etnsdatabase syntax for service name

_Note_ system/ORCLCDB@etnsdatabase syntax for sid

_Note_ user/password@ip:port/sid

_Note_ user/password@service-name

## Create backup from script

```bash
#!/bin/bash

# Get the current date and time in a human-readable format
DATE_TIME=$(date +"%Y-%m-%d_%H-%M-%S")

# Run the expdp command inside the container
expdp system/ORCLCDB@etnsdatabase FULL=Y \
  DUMPFILE=full_db_backup_$DATE_TIME.dmp \
  DIRECTORY=dump_dir \
  LOGFILE=expdp_full_db_$DATE_TIME.log
```

## DBA and PDB

In Oracle, **DBA** and **PDB** are terms that refer to different concepts in the context of **multitenant** architecture (introduced in Oracle 12c), which allows multiple **Pluggable Databases (PDBs)** to exist within a single **Container Database (CDB)**.

### 1. **DBA (Database Administrator)**

- **DBA** typically refers to a **Database Administrator** role, but in the context of Oracle, it can also refer to a view or set of views that are used to access system-level data in the database. The **DBA** prefix in Oracle views refers to **Data Dictionary Views** that provide information for database administrators about the entire database system, including all containers and pluggable databases.

  **Examples of DBA Views**:

  - `DBA_USERS`: Lists all users in the database.
  - `DBA_TABLES`: Lists all tables in all schemas of the database.
  - `DBA_TABLESPACES`: Lists all tablespaces in the database.

  These views are typically used by database administrators (DBAs) to manage and monitor the database system.

### 2. **PDB (Pluggable Database)**

- **PDB (Pluggable Database)** refers to a **sub-database** that resides within a **Container Database (CDB)** in Oracle’s **multitenant** architecture. The multitenant architecture allows a **CDB** to host multiple **PDBs**, which are essentially isolated databases that share the same instance and resources but operate independently. Each PDB has its own schema, users, tables, and other objects.

- **CDB** (Container Database) is the root database that contains one or more **PDBs**. A **CDB** includes:
  - **CDB$ROOT**: The root container that holds common metadata and system-level information.
  - **PDB$SEED**: A template pluggable database that is used for creating new PDBs.
  - **PDBs**: These are the actual pluggable databases created within the CDB.

#### Key Points about PDB:

- A **PDB** is a self-contained, user-created database.
- Each PDB can have its own users, schemas, and application data.
- PDBs can be unplugged from one CDB and plugged into another CDB.
- PDBs allow for easier database consolidation and management as multiple databases (PDBs) can share the same Oracle instance (CDB).

### Example:

- **CDB** (Container Database): This is like a large container that holds several databases inside it.
- **PDB** (Pluggable Database): Each PDB inside the CDB behaves like a separate, standalone database but shares the same underlying infrastructure.

### Visual Representation:

```
[ CDB ]
   ├── [ CDB$ROOT ]  (System and metadata)
   ├── [ PDB$SEED ]  (Template PDB for cloning)
   ├── [ PDB1 ]      (User-created PDB 1)
   └── [ PDB2 ]      (User-created PDB 2)
```

### Key Differences Between CDB and PDB:

| **Aspect**     | **CDB (Container Database)**                       | **PDB (Pluggable Database)**                       |
| -------------- | -------------------------------------------------- | -------------------------------------------------- |
| **Role**       | Root database that contains one or more PDBs.      | A self-contained database within a CDB.            |
| **Metadata**   | Holds system-level metadata.                       | Holds application-specific data.                   |
| **Users**      | Contains administrative users (e.g., SYS, SYSTEM). | Contains application users and schemas.            |
| **Management** | Managed by DBAs at the container level.            | Managed by users at the pluggable level.           |
| **Isolation**  | No isolation between PDBs.                         | Isolated, each PDB behaves as a separate database. |

### Conclusion:

- **DBA**: Refers to the administrative role and views in Oracle that provide information about the entire database (CDB and PDBs).
- **PDB**: A self-contained, pluggable database within a **CDB**. Each PDB operates independently but shares the same Oracle instance, making it efficient for managing multiple databases.

In a **multitenant architecture**, a **CDB** manages multiple **PDBs**, and the DBA's role is crucial in managing the entire system, including both the CDB and the PDBs within it.

## Create a PDB

```sql
# connect to db inside docker
sqlplus sys/ORCLCDB@//localhost:1521/etnsdatabase as sysdba

# show pdb
SELECT pdb_name, status FROM dba_pdbs ORDER BY pdb_name;
SELECT name, open_mode FROM v$pdbs ORDER BY name;

# create pdb
CREATE PLUGGABLE DATABASE ORCLPDB2 ADMIN USER etns_recon IDENTIFIED BY recon CREATE_FILE_DEST='/opt/oracle/oradata';

# enable read right mode
ALTER PLUGGABLE DATABASE ORCLPDB2 OPEN READ WRITE;
```

## Check PDB

### sql developer or within sql

```sql
# show all dba/pdb
SELECT name FROM v$containers;

# show current dba/pdb
SELECT sys_context('userenv', 'con_name') FROM dual;

# switch pdb
ALTER SESSION SET CONTAINER = ORCLPDB2;

# Set PDB as default in sql developer/spring
use PDB name as service name and you can clear sid
```

_Note_
https://oracle-base.com/articles/12c/multitenant-create-and-configure-pluggable-database-12cr1

## Error!!

Get inside the Oracle database container via the docker exec -it containerID bash command, you can check the logs to diagnose the issue. Here's what you should do:

1. Navigate to the Log Directory

Oracle logs are typically located in /opt/oracle/diag or a similar directory depending on the Oracle version. Run the following commands to locate the logs:
cd /opt/oracle/diag

If this directory doesn't exist, try:
cd /u01/app/oracle/diag

2. Identify the Database Instance

Navigate to the logs directory for the specific database instance. For example:
cd /u01/app/oracle/diag/rdbms/<database_name>/<instance_name>/trace

Replace <database_name> and <instance_name> with the names of your database and instance. If you're unsure, list the directories:
ls

3. View Alert Logs

The primary log file for Oracle database issues is the alert log. Check for it in the trace directory:
cat alert\_<instance_name>.log

If the log is large, use a pager like less or tail the last few lines:
less alert\_<instance_name>.log

# or

tail -n 100 alert\_<instance_name>.log

4. Check Listener Logs

If the database listener might be causing the issue, navigate to the listener logs:
cd /u01/app/oracle/diag/tnslsnr/<hostname>/listener/trace

View the listener log:
cat listener.log

# or

tail -n 100 listener.log

5. Check System Logs

In some cases, container-related issues might also cause problems. Check the container logs from outside the container:
docker logs 83287a12344

6. Debug Common Issues

If you find an error in the logs, look for specific messages or error codes. Common issues include:

ORA-xxxx errors: Database-related issues.
Listener errors: Network or connection problems.
Memory or disk space issues.

7. Check the Database Status

If the container is running but the database is not starting properly, try connecting to the database inside the container:
sqlplus / as sysdba

Once in SQL\*Plus, check the status:
SELECT INSTANCE_NAME, STATUS FROM V$INSTANCE;

If the database is not running, try starting it:
STARTUP;

initetnsdatabase.ora error!
The error messages indicate that the Oracle database instance cannot locate or open the initialization parameter file (initetnsdatabase.ora). This file contains essential configuration parameters for the database startup.

Steps to Resolve

1. Verify the Parameter File Path

Locate the File: Check if the file exists in the specified directory:
ls -l /opt/oracle/product/19c/dbhome_1/dbs/initetnsdatabase.ora

If the file exists but has incorrect permissions:
chmod 644 /opt/oracle/product/19c/dbhome_1/dbs/initetnsdatabase.ora

Search for Alternate Locations: If the file is not found, it might be in a different location. Search for it:
find /opt/oracle -name "initetnsdatabase.ora"

Copy or Link the File: If the file exists elsewhere, copy or create a symbolic link:
cp /path/to/initetnsdatabase.ora /opt/oracle/product/19c/dbhome_1/dbs/

2. Verify Database SID

Ensure that the Oracle SID is set correctly. Run:
echo $ORACLE_SID

If the output does not show etnsdatabase, set it:
export ORACLE_SID=etnsdatabase

3. Check the Initialization Parameter File

If the initetnsdatabase.ora file is missing or corrupted, recreate it. The file should contain a pointer to the spfile (server parameter file). For example:

Create a new initetnsdatabase.ora:
vi /opt/oracle/product/19c/dbhome_1/dbs/initetnsdatabase.ora

Add the following content:
SPFILE='/opt/oracle/product/19c/dbhome_1/dbs/spfileetnsdatabase.ora'

Replace the path with the actual location of the spfile.

4. Check for spfile

Verify the existence of the spfile:
ls -l /opt/oracle/product/19c/dbhome_1/dbs/spfileetnsdatabase.ora

If the spfile is missing, recreate it from the pfile (parameter file). Example:
sqlplus / as sysdba
CREATE SPFILE FROM PFILE='/path/to/pfile.ora';

5. Restart the Database

Once the parameter file issue is resolved, restart the database:

Log in as sysdba:
sqlplus / as sysdba

Start the database:
STARTUP;

Preventive Measures

Backup Configuration Files: Regularly back up your init.ora and spfile to avoid loss.
Monitor File Permissions: Ensure critical files have appropriate permissions to prevent accidental deletion or access issues.
Use a Consistent SID: Always set the correct SID in the environment variables.

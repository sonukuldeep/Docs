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
sqlplus system/ORCLCDB@//localhost:1521/etnsdatabase

# do not change
CREATE DIRECTORY dump_dir AS '/home/oracle/dump';

expdp system/ORCLCDB@etnsdatabase FULL=Y DUMPFILE=full_db_backup.dmp DIRECTORY=dump_dir LOGFILE=expdp_full_db.log

#make sure the dump and log files have oracle oinstall user and group
impdp system/ORCLCDB@etnsdatabase FULL=Y DUMPFILE=full_db_backup.dmp DIRECTORY=dump_dir LOGFILE=impdp_full_db.log

#!/bin/bash

# Get the current date and time in a human-readable format
DATE_TIME=$(date +"%Y-%m-%d_%H-%M-%S")

# Run the expdp command inside the container
expdp system/ORCLCDB@etnsdatabase FULL=Y \
  DUMPFILE=full_db_backup_$DATE_TIME.dmp \
  DIRECTORY=dump_dir \
  LOGFILE=expdp_full_db_$DATE_TIME.log


expdp system/ORCLCDB@etnsdatabase FULL=Y DUMPFILE=full_db_backup_$(date +"%d-%m").dmp DIRECTORY=dump_dir LOGFILE=expdp_full_db_$(date +"%d-%m").log
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

| **Aspect**             | **CDB (Container Database)**       | **PDB (Pluggable Database)**             |
|------------------------|------------------------------------|------------------------------------------|
| **Role**               | Root database that contains one or more PDBs. | A self-contained database within a CDB.  |
| **Metadata**           | Holds system-level metadata.       | Holds application-specific data.         |
| **Users**              | Contains administrative users (e.g., SYS, SYSTEM). | Contains application users and schemas.  |
| **Management**         | Managed by DBAs at the container level. | Managed by users at the pluggable level. |
| **Isolation**          | No isolation between PDBs.         | Isolated, each PDB behaves as a separate database. |

### Conclusion:

- **DBA**: Refers to the administrative role and views in Oracle that provide information about the entire database (CDB and PDBs).
- **PDB**: A self-contained, pluggable database within a **CDB**. Each PDB operates independently but shares the same Oracle instance, making it efficient for managing multiple databases.

In a **multitenant architecture**, a **CDB** manages multiple **PDBs**, and the DBA's role is crucial in managing the entire system, including both the CDB and the PDBs within it.

## Create a PDB
### connect to db inside docker
sqlplus sys/ORCLCDB@//localhost:1521/etnsdatabase as sysdba

### show pdb
SELECT pdb_name, status FROM dba_pdbs ORDER BY pdb_name;
SELECT name, open_mode FROM v$pdbs ORDER BY name;

### create pdb
CREATE PLUGGABLE DATABASE ORCLPDB2 ADMIN USER etns_recon IDENTIFIED BY recon CREATE_FILE_DEST='/opt/oracle/oradata';

### enable read right mode
ALTER PLUGGABLE DATABASE ORCLPDB2 OPEN READ WRITE;

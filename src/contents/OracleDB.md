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

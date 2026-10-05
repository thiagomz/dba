# Create OEM1 Database (dbca)

```
grep dummy /etc/oratab
dummy:/u01/app/oracle/product/dbhome_1931:N
. oraenv
dummy
```

### Create database without a PDB on filesystem

```
dbca -silent -createDatabase                                             \
-templateName General_Purpose.dbc                                        \
-gdbname OEM1 -sid  OEM1 -responseFile NO_VALUE                          \
-characterSet AL32UTF8                                                    \
-sysPassword SysPassword1                                                 \
-systemPassword SysPassword1                                             \
-createAsContainerDatabase false                                          \
-numberOfPDBs 0                                                           \
-databaseType MULTIPURPOSE                                                \
-memoryMgmtType auto_sga                                                  \
-totalMemory 2000                                                         \
-storageType FS                                                           \
-datafileDestination /u01/app/oracle/oradata                             \
-redoLogFileSize 50                                                       \
-emConfiguration NONE                                                     \
-ignorePreReqs

```
Expected Output

```
Prepare for db operation
10% complete
Copying database files
40% complete
Creating and starting Oracle instance
42% complete
46% complete
50% complete
54% complete
60% complete
Completing Database Creation
66% complete
69% complete
70% complete
Executing Post Configuration Actions
100% complete
Database creation complete. For details check the logfiles at:
 /u01/app/oracle/cfgtoollogs/dbca/OEM1.
Database Information:
Global Database Name:OEM1
System Identifier(SID):OEM1
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/OEM1/OEM1.log" for further details.
```


Change N to Y
```
grep OEM1 /etc/oratab
OEM1:/u01/app/oracle/product/dbhome_1931:Y
. oraenv
OEM1
```

### Start Listener

```
$ lsnrctl start
```

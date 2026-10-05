### Install Oracle 19.31

Setup directory

```
mkdir -p /u01/app/oracle/{oradata,fra,product}
chown -R oracle:oinstall /u01
chmod -R 775 /u01
```

Create installation file for 19.31 Autoupgrade.

```
cat << 'EOF' > /home/oracle/autoupgrade/install-home-19.31.cfg
# =====================================================================
# AutoUpgrade Configuration File - Home Creation & Patching
# Target: Oracle Database 19c Enterprise Edition (Linux x86-64)
# =====================================================================

global.global_log_dir=/home/oracle/autoupgrade/logs

# Apontamento exato da pasta onde residem os ZIPs dos patches
install1.folder=/media/sf_OracleUpgrade/patches
install1.target_home=/u01/app/oracle/product/dbhome_1931
install1.target_version=19
install1.home_settings.edition=EE
install1.home_settings.oracle_base=/u01/app/oracle
install1.home_settings.inventory_location=/u01/app/oraInventory
install1.download=no

# Notação explícita de atualização exigida pelo AutoUpgrade
install1.patch=RU:19.31,OPATCH,OJVM,DPBP
EOF
```

```
cd /home/oracle/autoupgrade

java -jar autoupgrade.jar -config install-home-19.31.cfg -patch -mode create_home
```

Expected Output

```
[oracle@oraoem01 autoupgrade]$ java -jar autoupgrade.jar -config install-home-19.31.cfg -patch -mode create_home
AutoUpgrade Patching 26.5.260807 launched with default internal options
Processing config file ...
+-----------------------------------------+
| Starting AutoUpgrade Patching execution |
+-----------------------------------------+
Type 'help' to list console commands
patch> lsj -a 10
+----+-------------+-------+---------+-------+----------+-------+---------------------+
|Job#|      DB_NAME|  STAGE|OPERATION| STATUS|START_TIME|UPDATED|              MESSAGE|
+----+-------------+-------+---------+-------+----------+-------+---------------------+
| 100|create_home_1|EXTRACT|EXECUTING|RUNNING|  11:08:18|14s ago|Extracting Gold Image|
+----+-------------+-------+---------+-------+----------+-------+---------------------+
Total jobs 1

The command lsj is running every 10 seconds. PRESS ENTER TO EXIT
.
.
.
The command lsj is running every 10 seconds. PRESS ENTER TO EXIT
Job 100 completed
------------------- Final Summary --------------------
Number of databases            [ 1 ]

Jobs finished                  [1]
Jobs failed                    [0]
Jobs restored                  [0]
Jobs pending                   [0]

Please check the summary report at:
/home/oracle/autoupgrade/logs/cfgtoollogs/patch/auto/status/status.html
/home/oracle/autoupgrade/logs/cfgtoollogs/patch/auto/status/status.log

```

Add to /etc/oratab for easy Environment configuration

```
dummy:/u01/app/oracle/product/dbhome_1931:N

. oraenv
dummy
```
Verify Oracle Database Software Version and Patches

```
cd $ORACLE_HOME/OPatch

[oracle@oraoem01 OPatch]$ ./opatch lspatches
39196236;DATAPUMP BUNDLE PATCH 19.31.0.0.0
38906621;OJVM RELEASE UPDATE: 19.31.0.0.260421 (38906621)
39833225;DISABLE ALIAS SCAN PERFORMANCE IMPROVEMENT IN BUG-38594261
39779336;Fix for Bug 39779336
39761126;Fix for Bug 39761126
39761039;Fix for Bug 39761039
39750798;Fix for Bug 39750798
39661114;Fix for Bug 39661114
39661105;Fix for Bug 39661105
39661089;Fix for Bug 39661089
39598920;MERGE ON DATABASE RU 19.31.0.0.0 OF 39060101 39127997 39128024
39594515;Fix for Bug 39594515
39575331;Fix for Bug 39575331
39570736;Fix for Bug 39570736
39570655;Fix for Bug 39570655
39498075;MERGE ON DATABASE RU 19.31.0.0.0 OF 39360889 39370763
39487170;Fix for Bug 39487170
39429942;  DB_DOMAIN NAME CHANGED TO UPPER CASE AUTOMATICALLY
39290842;Fix for Bug 39290842
39234501;Fix for Bug 39234501
39060241;Fix for Bug 39060241
39052286;Fix for Bug 39052286
38937302;AFTER APPLYING 19.30 DBRU THE AUTOTASK SQL TUNING ADVISOR GETS ORA-13602
38494116;Fix for Bug 38494116
39039430;OCW RELEASE UPDATE 19.31.0.0.0 (39039430)
39034528;Database Release Update : 19.31.0.0.260421 (REL-APR2026) (39034528)

OPatch succeeded.

```

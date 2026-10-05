# Autoupgrade Download

#### Download autoupgrade.jar
```
mkdir -p /home/oracle/autoupgrade/{logs,keystore}
cd /home/oracle/autoupgrade
curl -L -O https://download.oracle.com/otn-pub/otn_software/autoupgrade.jar
```
#### Setup MOS Login

> Create a download file

```
cat << 'EOF' > /home/oracle/autoupgrade/get-patch-19c.cfg
global.global_log_dir=/home/oracle/autoupgrade/logs
global.keystore=/home/oracle/autoupgrade/keystore
global.folder=/media/sf_OracleUpgrade/patches

patch1.platform=LINUX.X64
patch1.patch=RU:19.31,OPATCH,OJVM

patch2.platform=LINUX.X64
patch2.patch=RU:19.32,OPATCH,OJVM
EOF
```

> Configure Wallet to access MOS and download files

```
$ java -jar autoupgrade.jar -config get-patch-19c.cfg -patch -load_password

-> SysPassword1

MOS> add -user YouMOSUser@e-mail.com

-> Password MOS user
-> save
-> yes
-> exit
```

> Execute the download

```
$ java -jar autoupgrade.jar -config get-patch-19c.cfg -patch -mode download

```

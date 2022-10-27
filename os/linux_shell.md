# MySQL Shell Scripts

## Load Backup
```
#!/bin/bash
# MySQL load script
# Copyright (c) 2018 SideAI
# ---------------------------

#scp red@192.168.31.70:/mnt/sidepool/backup/pdrp-231/mysql/backup-pdbs067/2020-10-28.04.07.tar.gz /root/mysql
#tar -zxvf 2020-10-28.04.23.tar.gz

echo "#####################################"
echo "USAGE: /usr/bin/bash load.sh [:backup_path] [:db_name]"
echo "#####################################"

#########################
######TO BE MODIFIED#####

### MySQL Connection Setup ###
#For MYSQL 5.6+, please to use mysql_config_editor for better security.
#mysql_config_editor set --host=localhost --user=db_user --password

#If you already set encrypted your login via mysql_config_editor, just leave MUSER/MPASS/MHOST empty.
MUSER=""
MPASS=""
MHOST=""

######DO NOT MAKE MODIFICATION BELOW#####
#########################################

echo "###### MYSQL LOAD START ######"
date +"%Y-%m-%d.%H.%M.%S"

### Binaries ###
MYSQL="$(which mysql)"

if [ -z "$1" ] || [ -z "$2" ];then
	echo "Missing Parameters..."
	exit
fi

BACKUP_PATH=$1
MDBNAME=$2

BACKUP_FILES="$(ls $BACKUP_PATH | grep .sql)"
BACKUP_FILES_ARR=($BACKUP_FILES)

TOTAL=${#BACKUP_FILES_ARR[@]}
IDX=1
for FILE in "${BACKUP_FILES_ARR[@]}";do
	echo "[$IDX/$TOTAL]Loading File [$FILE] ..."
	((IDX++))
	if [ -z "$MUSER" ] || [ -z "$MHOST" ] || [ -z "$MPASS" ]; then
		$MYSQL --login-path=local $MDBNAME < $BACKUP_PATH/$FILE
	else
		$MYSQL -u $MUSER -h $MHOST -p$MPASS $MDBNAME < $BACKUP_PATH/$FILE
	fi
done

date +"%Y-%m-%d.%H.%M.%S"
echo "###### MYSQL LOAD END ######"
```

Dump Backup
```
#!/bin/bash
# MySQL dump script
# Copyright (c) 2018 SideAI
# ---------------------------

echo "#####################################"
echo "USAGE: /usr/bin/bash dump.sh [:dump_path] [:db_name]"
echo "#####################################"

#########################
######TO BE MODIFIED#####

### MySQL Connection Setup ###
#For MYSQL 5.6+, please to use mysql_config_editor for better security.
#mysql_config_editor set --host=localhost --user=db_user --password

#If you already set encrypted your login via mysql_config_editor, just leave MUSER/MPASS/MHOST empty.
MUSER=""
MPASS=""
MHOST=""

######DO NOT MAKE MODIFICATION BELOW#####
#########################################

if [ -z "$1" ] || [ -z "$2" ];then
	echo "Missing Parameters...EXIT!"
	exit
fi

echo "###### MYSQL DUMP START ######"
date +"%Y-%m-%d.%H.%M.%S"

DUMP_PATH=$1
DB_NAME=$2

mkdir -p $DUMP_PATH

if [ -z "$MUSER" ] || [ -z "$MHOST" ] || [ -z "$MPASS" ]; then
	for DB_TABLE in $(mysql --login-path=local -D $DB_NAME -e 'SHOW TABLES' | grep -v Tables_in_)
	do
		echo "DUMPING TABLE: [$DB_NAME].[$DB_TABLE]"
		mysqldump --login-path=local $DB_NAME $DB_TABLE > $DUMP_PATH/$DB_TABLE.sql
	done
else
	for DB_TABLE in $(mysql -u $MUSER -h $MHOST -p$MPASS -D $DB_NAME -e 'SHOW TABLES' | grep -v Tables_in_)
	do
		echo "DUMPING TABLE: [$DB_NAME].[$DB_TABLE]"
		mysqldump -u $MUSER -h $MHOST -p$MPASS $DB_NAME $DB_TABLE > $DUMP_PATH/$DB_TABLE.sql
	done
fi

date +"%Y-%m-%d.%H.%M.%S"
echo "###### MYSQL DUMP END ######"
```

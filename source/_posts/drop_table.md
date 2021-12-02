
```
#!/bin/bash

DROP_DATABASE=${1:-"some-db"}
AISTACK_VIP=${2:-"xxx.xxx.xxx.xxx"}
AISTACK_MYSQL_ROOT_PASSWORD=${3:-"R0otAwc10ud"}
AISTACK_MYSQL_PORT=${4:-"30090"}


function help_info {
    echo "[HELP]  Auto drop all tables in target database"
    echo "Useage: bash ${0##*/} db_name aistack_vip [aistack_root_password] [aistack_mysql_port]"
    echo "        bash ${0##*/} ailab 10.1.0.200"
    echo "        bash ${0##*/} ailab 10.1.0.200 R0otAwc10ud 30090"
    echo "        bash ${0##*/} ailab 10.1.0.200 R0otAwc10ud 30090 \"config|tb_sysconfig\""
    echo "command options:"
    echo "        db_name: which database you want to drop"
    echo "        aistack_vip: external IP address of aistack cluster"
    echo "        aistack_root_password: aistack mysql root password"
    echo "        aistack_mysql_port: aistack mysql external port"
    echo "        skip_drop_tables: skip drop tables you want"

}

if [ $AISTACK_VIP == 'xxx.xxx.xxx.xxx' ]; then
    echo -e '[ERROR] Please set external IP address.\n' >&2
    help_info
    exit 1
fi

if [ $DROP_DATABASE == 'some-db' ]; then
    echo -e '[ERROR] Please set drop DATABASE NAME.\n' >&2
    help_info
    exit 1
fi

mysql -P$AISTACK_MYSQL_PORT -h$AISTACK_VIP -uroot -p$AISTACK_MYSQL_ROOT_PASSWORD -Nse 'show tables' $DROP_DATABASE | while read table; do mysql -P$AISTACK_MYSQL_PORT -h$AISTACK_VIP -uroot -p$AISTACK_MYSQL_ROOT_PASSWORD -e "SET FOREIGN_KEY_CHECKS = 0; drop table $table; SET FOREIGN_KEY_CHECKS = 1" $DROP_DATABASE; echo "drop table $table"; done
```

# «Резервное копирование баз данных»

## Задание 1. Резервное копирование  
Кейс  
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.  
Необходимо описать, какие варианты резервного копирования подходят в случаях:  

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.  
1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.  

## Ответ  

1.1 В таких случаях подойдет ежедневное полное резервное копирование. Все данные базы данных копируются в конце дня в нерабочее время и хранятся на отдельных носителях. В случае сбоя можно быстро восстановить данные. Желательно иметь еще одну копию бэкапа в независимом месте. Как показывает практика , иногда срабатывает закон Мерфи, и всед за умершим дискомм с БД, через 20 минут скачком напряжения убивает сервер с бэкапом и на восстановление уже уходит гораздо больше времени. 
 
1.2 Основой является полный бэкап. С необходимой частотой (в данном случае раз в час)  делается  инкрементный бэкап. Для восстановления данных необходимо восстановить данные на начало дня полным бэкапом, а далее восстанавливать данные инкрементными бэкапами до актуального состояния. 


## Задание 2. PostgreSQL  
2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).  

pg_dump -Fc mydb > db.dump  

Восстановление данных в целевую БД  
pg_restore -v --no-owner --host=<server name> --port=<port> --username=<user-name> --dbname=<target database name> <database>.dump  
  

## Задание 3. MySQL  
3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL.  
Инкрементное резервное копирование в MySQL обычно выполняется с использованием утилиты mysqlbackup из MySQL Enterprise Backup (которая является платной) или с помощью бинлогов (binary logs) в стандартной версии MySQL.  

mysqlbinlog --start-datetime="2023-03-01 00:00:00" \  
            --stop-datetime="2023-03-02 00:00:00" \  
            --to-last-log \  
            --result-file="/path_to_backup/incremental_backup.sql" \  
            binlog.000001 binlog.000002 ...  

 

FROM mysql:5.7.25

ENV MYSQL_DATABASE="accounts"

COPY db_backup.sql docker-entrypoint-initdb.d/db_backup.sql

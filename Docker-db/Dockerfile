FROM mariadb:lts-noble

ENV MYSQL_DATABASE="accounts"
ENV MARIADB_ROOT_PASSWORD_FILE=/run/secrets/db_pass

COPY db_backup.sql docker-entrypoint-initdb.d/db_backup.sql

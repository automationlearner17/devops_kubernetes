FROM mysql:5.7
LABEL "app"="backend"

COPY db.sql /docker-entrypoint-initdb.d/db.sql
EXPOSE 3306


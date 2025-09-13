# 🔧 docker-apache-php-mysql

Local development environment containerized with Docker, configured with **Apache**, **PHP**, and **MySQL**.
Designed for traditional PHP projects, this setup lets you work professionally without installing local tools like MAMP or XAMPP.

---

## 🚀 Features

- ✅ Apache with `.htaccess` support
- ✅ Ready-to-use PHP (version configurable)
- ✅ MySQL with a persistent volume
- ✅ No dependency on local installations
- ✅ Quick start with Docker Compose

---

## 📦 Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

---

## 🛠️ Quick start

1. Clone the repository:
   ```bash
   git clone https://github.com/Areasettantotto/docker-apache-php-mysql.git
   cd docker-apache-php-mysql
   ```

---

## Local setup notes

- Create the runtime log file so PHP can write errors locally:

   ```bash
   mkdir -p logs
   touch logs/php_errors.log
   chmod 664 logs/php_errors.log
   ```

- Place your database dump inside the `mysql/` directory as `dump.sql` if you want the project to initialize or re-import the database locally. The repo ignores `mysql/dump.sql` by design; keep a local copy if you need it.

- Configure your local `.env` file (copy from `.env.example`) and set the DB name to match the dump (for example `MYSQL_DATABASE=my_db`):

   ```properties
   MYSQL_ROOT_PASSWORD=root
   MYSQL_DATABASE=my_db
   MYSQL_USER=root
   MYSQL_PASSWORD=root
   ```


   ```bash
   docker compose up -d --build
   docker compose run --rm db-init
   docker compose exec db mysql -uroot -proot -e "USE my_db; SHOW TABLES; SELECT COUNT(*) FROM example;"

## Maintenance

If you need to recreate the database from scratch (destructive), use the following commands. These will DROP the existing database and re-import the dump from `mysql/dump.sql`.

Warning: this is destructive and will permanently delete the data in the specified database. Make a backup first if needed.

```bash
# bring the stack up
docker compose up -d --build

# drop and recreate the database, then import the dump (safe single command)
docker compose exec db mysql -uroot -proot -e "DROP DATABASE IF EXISTS my_db; CREATE DATABASE my_db;"
docker compose exec db bash -lc "mysql -uroot -proot my_db < /docker-entrypoint-initdb.d/dump.sql"

# verify
docker compose exec db mysql -uroot -proot -e "USE my_db; SHOW TABLES; SELECT COUNT(*) FROM example;"
```

Alternatively you can use the included helper service which waits for the DB to be healthy and imports the dump safely:

```bash
docker compose run --rm db-init
```

   ```


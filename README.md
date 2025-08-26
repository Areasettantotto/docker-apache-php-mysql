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

- To import the dump into a running DB (safe, non-destructive to other DBs), run the helper service included in `docker-compose.yml`:

   ```bash
   docker compose up -d --build
   docker compose run --rm db-init
   docker compose exec db mysql -uroot -proot -e "USE my_db; SHOW TABLES; SELECT COUNT(*) FROM example;"
   ```


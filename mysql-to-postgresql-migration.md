## Migrating data from MySQL to PostgreSQL

Migrating data from `MySQL` to `PostgreSQL` can be very challenging.

### Problems that come with it

- PostgreSQL does **not support MySQL-specific comments**, especially versioned comments like:
  ```sql
  /*!40101 SET @OLD_SQL_MODE=@@SQL_MODE */
  ```

- PostgreSQL does not use `AUTO_INCREMENT`. Instead, it relies on **sequences** or **identity columns**.

- Some data types and structures must be converted:
  - `tinyint(1)` → `boolean`
  - `datetime` → `timestamp`
  - `longtext` → `text`

- Index names and constraints may differ because PostgreSQL has stricter rules.

That is the main reason why specialized tools were created to perform the migration while converting the data.

---

### Important note (NB)

We cannot directly use a MySQL/MariaDB exported dump file with PostgreSQL.
The SQL syntax is different, so the dump must be converted and normalized before it can be imported.

To solve this problem, the tool **pgloader** was created.

Its goal is quite simple:

- Connect to the source database in live mode (without relying on a dump file)
- Read table structures and data
- Convert them into a PostgreSQL-compatible format
- Import everything into the target PostgreSQL database

---

### Example usage with pgloader

#### Install the tool

```bash
sudo apt search pgloader
```

You should see something like:

```
pgloader/kali-rolling 3.6.10-5 amd64
extract, transform and load data into PostgreSQL
```

---

#### Memory configuration (IMPORTANT)

If your database is larger than **1 GB**, pgloader may crash due to memory limits.
This is because pgloader runs on **SBCL (Lisp runtime)**.

Set a larger heap size before running it:

```bash
export SBCL_DYNAMIC_SPACE_SIZE=4096
# or, if you have enough RAM:
# export SBCL_DYNAMIC_SPACE_SIZE=8192
```

---

#### Run the migration

```bash
#    --with "workers = 1"
pgloader   --with "batch rows = 1000"   --with "prefetch rows = 1000"   mysql://sourceDBusername:sourceDBuserPassword@localhost/source_database   postgresql://targetDBUsername:targetDBuserPassword@localhost/targetDB
```

**Notes:**

- `workers = 1` is safer for large databases (default is 4)
- Smaller batch sizes reduce memory pressure
- pgloader may appear silent for several minutes — this is normal

---

### Using a `.load` file (alternative approach)

There is another technique that I tried.
It works, but it is not always necessary unless you want more control.

#### 1. Create a `migrate.load` file

#### 2. Add the following instructions

```lisp
LOAD DATABASE
    FROM mysql://sourceDBusername:sourceDBuserPassword@localhost/source_database
    INTO postgresql://targetDBUsername:targetDBuserPassword@localhost/targetDB

WITH include drop,
     create tables,
     create indexes,
     reset sequences,
     batch rows = 1000,
     prefetch rows = 1000,
     workers = 1

SET search_path TO 'public';
```

This approach allows:

- Better control over the migration process
- Explicit schema handling
- Cleaner PostgreSQL output

---

#### 3. Start the migration

```bash
export SBCL_DYNAMIC_SPACE_SIZE=4096
pgloader migrate.load
```

---

### Final notes

- pgloader usually creates one PostgreSQL schema per MySQL database
- Applications like **Laravel** rely on PostgreSQL’s `search_path` to know which schema to use (check the config section)
- After migration, always verify:
  - Table counts
  - Indexes
  - Sequences (AUTO_INCREMENT equivalents)

---

## Conclusion

Migrating from MySQL to PostgreSQL is not a simple import.
It is a **conversion process**, and tools like **pgloader** make it possible — but they still require
proper configuration and understanding.


---- 


#### Config Section

```bash
# .env 
DB_CONNECTION=pgsql
DB_HOST=postgres
DB_PORT=5432
DB_DATABASE=app
DB_SCHEMA=custom_schema
DB_USERNAME=laravel
DB_PASSWORD=secret
```

```bash
# config/database.php
    'pgsql' => [
        'driver' => 'pgsql',
        'url' => env('DATABASE_URL'),
        'host' => env('DB_HOST', '127.0.0.1'),
        'port' => env('DB_PORT', '5432'),
        'database' => env('DB_DATABASE', 'forge'),
        'username' => env('DB_USERNAME', 'forge'),
        'password' => env('DB_PASSWORD', ''),
        'charset' => 'utf8',
        'prefix' => '',
        'prefix_indexes' => true,
        'search_path' => env('DB_SCHEMA', 'public'), // Added to support custom schema
        'sslmode' => 'prefer',
    ],
```
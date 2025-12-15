# MongoDB – Import Compressed Database Backup

This file documents **common MongoDB restore scenarios**, from simple local restores to Docker-based workflows.
Duplication has been removed and commands are grouped by **use case**.

---

## 1. Test connection to MongoDB

```bash
# Think of it like: "I verify that I can reach the MongoDB server"
mongosh \
  --host 172.24.0.4 \
  --port 27017 \
  -u username \
  -p password \
  --authenticationDatabase admin
```

---

## 2. Restore database collections (non-compressed)

### Simple mode – without credentials

```bash
# Think of it like: "Restore a local dump without authentication"
mongorestore \
  --db db_name_staging \
  $HOME/trash/mongodbs/analytic
```

---

### Simple mode – with credentials

```bash
# Think of it like: "Restore a dump using database credentials"
mongorestore \
  --username username \
  --password password \
  --authenticationDatabase admin \
  --db db_name_staging \
  /db_backups_path/
```

---

### Restore only specific namespaces (collections)

```bash
# Think of it like: "Restore only selected databases or collections"
mongorestore \
  --username username \
  --password password \
  --authenticationDatabase admin \
  --nsInclude="app_name_test_mongo.*" \
  /db_backups/app_name_prod_mongo/
```

---

## 3. Restore a compressed backup (gzip)

```bash
# Think of it like: "Uncompress and restore a backup in one stream"
# Think of it like: "Uncompress and restore a backup in one stream (targeting a specific database)"
gunzip -c dump.gz | mongorestore \
  --username user \
  --password pass \
  --authenticationDatabase admin \
  --db db_name_target \
  --archive \
  --gzip
```

---

## 4. Restore into MongoDB running in Docker

### Stream a compressed dump into a container

```bash
# Think of it like: "Pipe the backup directly into the MongoDB container"
# Think of it like: "Pipe the compressed backup into the MongoDB container (specific DB)"
gunzip -c dump.gz | docker exec -i mongo_container mongorestore \
  --username user \
  --password pass \
  --authenticationDatabase admin \
  --db db_name_target \
  --archive \
  --gzip
```

---

### Restore from a mounted backup directory

> The backup directory must be **mounted inside the container**.

```bash
# Think of it like: "Restore from a path already visible inside the container"
docker exec -i mongo_container mongorestore \
  --username user \
  --password pass \
  --authenticationDatabase admin \
  --db app_name_test_mongo \
  /path/to/dump
```

---

### Real-world example (application container)

```bash
# Think of it like: "Restore production backup into a test database"
docker exec -i laravel_app_mongodb mongorestore \
  --username username \
  --password password \
  --authenticationDatabase admin \
  --db app_prod_api \
  db_backups/app_name_prod_mongo
```

---

## 5. Lab / testing restore

```bash
# Think of it like: "Quick restore for local testing"
mongorestore \
  --username username \
  --password password \
  --authenticationDatabase admin \
  --db app_name_test_mongo \
  /db_backups_path/
```

---

## Mental checklist

```bash
# Think of it like:
# 1. mongosh        → test connectivity
# 2. mongorestore  → choose restore mode
# 3. gzip | docker → adapt to compression & containers
# 4. --db / --ns   → control restore scope
```


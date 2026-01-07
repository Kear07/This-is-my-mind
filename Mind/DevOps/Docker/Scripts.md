------------------------
### Inicialização SQL e NOSQL

#### SQL

Postgres
```sql
docker run --name Estudos_postgres -p 5432:5432 \
  -e POSTGRES_PASSWORD=1234 \
  -d postgres
```
obs: Usar o "-d" fará com que o container rode em 2° plano, se deseja acessa-lo pelo terminal, retire.


MySQL
```sql
docker run --name Estudos_mysql -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=1234 \
  -d mysql
```
obs: Usar o "-d" fará com que o container rode em 2° plano, se deseja acessa-lo pelo terminal, retire.

-----------------
#### NOSQL

Mongo
```sql
docker run --name Estudos_mongo -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=1234 \
  -d mongo
```
obs: Usar o "-d" fará com que o container rode em 2° plano, se deseja acessa-lo pelo terminal, retire.

Redis
```sql
docker run --name Estudos_redis -p 6379:6379 \
  -e REDIS_PASSWORD=1234 \
  -d redis
``` 
obs: Usar o "-d" fará com que o container rode em 2° plano, se deseja acessa-lo pelo terminal, retire.
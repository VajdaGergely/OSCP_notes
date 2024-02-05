# Postgresql
## info
* common port: 5432/tcp, 5433/tcp
* default database name: template1
## source
* https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql
## scanning and enumeration
* msf6> use auxiliary/scanner/postgres/postgres_version
* msf6> use auxiliary/scanner/postgres/postgres_login
* msf6> use auxiliary/scanner/postgres/postgres_hashdump
* msf6> use auxiliary/scanner/postgres/postgres_schemadump
## connect to db
<pre>
$ psql -U user1 (connect db on localhost, e.g. on target machine locally)
$ psql -h 10.10.10.3 -U user1 -d database_name (the default name is 'template1')
$ psql -h 10.10.10.3 -p port_num -U user1 -W Password123 database_name
</pre>
## db enumeration
<pre>
\list # List databases
\c <database> # use the database
\d # List tables
\du+ # Get users roles

# Get current user
SELECT user;

# Get current database
SELECT current_catalog;

# List schemas
SELECT schema_name,schema_owner FROM information_schema.schemata;
\dn+

#List databases
SELECT datname FROM pg_database;

#Read credentials (usernames + pwd hash)
SELECT usename, passwd from pg_shadow;

# Get languages
SELECT lanname,lanacl FROM pg_language;

# Show installed extensions
SHOW rds.extensions;
SELECT * FROM pg_extension;

# Get history of commands executed
\s
</pre>

## ========== upgrade a postgres cluster ======

https://gist.github.com/johanndt/6436bfad28c86b28f794

Another option

https://gist.github.com/Clark-G/01fcb84c658c44dcec4af1a965e2c3b4

## ========== install postgis   ============= ##

https://computingforgeeks.com/how-to-install-postgis-on-ubuntu-debian/

## =========SET UP================================
The installation procedure created a user account called postgres that is associated with the default Postgres role. In order to use Postgres, we'll need to log into that account. You can do that by typing:

sudo -i -u postgres
You will be asked for your normal user password and then will be given a shell prompt for the postgres user.

You can get a Postgres prompt immediately by typing:

psql

create user username with password 'password';
create database databasename with owner username;

OR IF DATABASE EXISTS:

GRANT ALL PRIVILEGES ON DATABASE databasename TO username;


log in 

psql -d databasename -U username

## ============ ALLOW local connections to PostgreSQL ============

> Run the psql command from the postgres user account:
sudo -u postgres psql postgres

> Set the password
\password postgres

> Close psql
\q

> open /etc/postgresql/9.4/main/pg_hba.conf (Ubuntu) 

> change ident to trust
~~~~~~~~~
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident
~~~~~~~~~~

Using trust allows the all local users to connect to the database without a password. This is convenience, but insecure. If you want a little more security, replace trust with md5, and use the password you set in the previous section to connect.

> Restart PostgreSQL:
sudo service postgresql restart

> now create a user as superuser for your account so you can psql from it in the example below you are user admin

sudo -u postgres createuser admin -s  
    
> finally set the default database to connect to when you type psql
> in this example its postgres    
> add the following to .bashrc or .zshrc

export PGDATABASE='postgres'


## ================unadvised way to setup================

We can create a new role USING INTERACTIVE by typing:

createuser -i


make the database 

createdb test1

create unix user to autolink to postgre

adduser test1


log into postgres
sudo -i -u test1
psql



## ===========SHOW CONNECTION INFORMATION============
\conninfo

SELECT * from pg_stat_activity;  #show active users connected
SELECT * from pg_user; #list all users
ALTER USER johndoe WITH CONNECTION LIMIT 2;  #number of smulaneous connections
ALTER USER django CREATEDB;   # make a user be able to create a database


SELECT rolname, rolconnlimit FROM pg_roles #shows allowed conn limit per user\d


====SWITCH DATABASE=======

psql -d databasename

OR 

\connect DBNAME   #from within the psql commandline

## ===========CREATE AND DESTROY THINGS================

CREATE TABLE table_name (
    column_name1 col_type (field_length) column_constraints,
    column_name2 col_type (field_length),
    column_name3 col_type (field_length)
);

INSERT INTO table_name (col-name-1, col-name-2, col-name-3) VALUES ('data1', 'data2', 'data3');  #Creates a row in the table

DELETE FROM table_name WHERE col-name = 'data value';      #deletes a row from the database where col name is specified 


ALTER TABLE table_name ADD column_name (datatype);	#creates a new column
ALTER TABLE table_name DROP column_name;		#drops a column

DROP TABLE table_name;					#drops a table




## ===========EDIT THINGS================


UPDATE playground SET type = 'new entry' WHERE color = 'yellow';

ALTER DATABASE test1 OWNER TO test1;


## =========== intert multiple rows========
INSERT INTO table_name (date_created, date_updated, created_by_id, updated_by_id, currency, amount, availability_status, company_id) 
SELECT (now() at time zone 'utc'), (now() at time zone 'utc'), 457, 457, 'ZAR', 5, 0, 184
FROM generate_series(1,5)

"2015-03-27 12:56:44.396948+00"
"2015-07-15 08:24:44.820222+00"

### ==============SEE THINGS===============

\conninfo		#show username, database, and port

\list #lists all databases
\dt #lists all tables in the current database
\du #list all users

SELECT * FROM table_name;	#shows all columns from table

## ===============RESTORE DATABASE=================

Restoring is pretty easy:

psql -U <username> -d <dbname> -1 -f <filename>.sql
or

/usr/bin/pg_restore --host localhost --port 5432 --username "name" --dbname "databasename" --no-password  --verbose "/home/tawanda/Downloads/mydb.backup"


Sometimes the role that was dumped with is not the role on the target server, you will get an error like


`pg_restore: error: could not execute query: ERROR:  role "postgres" does not exist`

in that case use the `--no-owner` option, this stops pg_restore trying to set the ownership of the objects to the original owner. Instead the objects will be owned by the user specified by `--role`

e.g. restoring a database on RDS

`pg_restore --no-owner --role=ebroot -h xxxx.rds.amazonaws.com -p 5432 -v -U ebroot -d ebdb mydb.dump`



## =============== QUERYING =======

SELECT COUNT(*) FROM  tablename; #counts rows in a table

SELECT column FROM tablename LIMIT 5;  #limits results to 5

> alternative ASC
SELECT first_name FROM accounts_user ORDER BY date_created DESC LIMIT 50; 



## =============== PUBLIC SCHEMA =========

> delete everything in the database but retain the database user and password

drop schema public cascade;
create schema public;

## ============== PRIMARY KEY IDENT ============
REF: https://www.postgresql.org/docs/current/functions-sequence.html

If you create a column as serial PostgreSQL automatically creates a sequence for that.

The name of the sequence is autogenerated and is always `tablename_columnname_seq` or `public.tablename_columnname_seq`, in your

if you have a table units_uom, then that sequence will be `units_uom_id_seq` or `public.units_uom_id_seq`
    
    
| Cant get the sequence name, no problem just list all the ones in the database `SELECT c.relname FROM pg_class c WHERE c.relkind = 'S';`    


To get the actual sequence name use

`select pg_get_serial_sequence('units_uom', 'id');`

to see what the current sequence value is:
```
SELECT  nextval('public.units_uom_id_seq');
SELECT currval('public.units_uom_id_seq');
```
You have to call nextval which will actually advance the value by 1

To set the sequence current value to a new number

Next item created will have identity 2
```
SELECT setval('public.units_uom_id_seq', 1);           
```

** NB: in djanog admin the ID's were not reset with django-import-export (edge case?)




# PHP Database Migrations Cookbook

In this repository, I have collected the most common examples of working with database migrations. For examples, I will use the syntax for writing migrations from the most popular tools for creating them:

* [Phinx](https://phinx.org/),  [documentation](http://docs.phinx.org/en/latest/)
* [Laravel Database Migrations](https://laravel.com/docs/master/migrations)

All examples will be accompanied by examples of native [MySQL](https://www.mysql.com/) queries.

## Table of Contents

* [Schema](#schema)
    * [Creating a scheme](#creating-a-database)
    * [Dropping a database](#dropping-a-database)

## Schema

### Creating a database

* check if the database is exists
* set charset
* set collation

MySQL

```mysql
CREATE DATABASE IF NOT EXISTS `cookbook` CHARACTER SET `utf8mb4` COLLATE `utf8mb4_unicode_ci`
```

Phinx (no special syntax)

```php
$this->execute('CREATE DATABASE IF NOT EXISTS `cookbook` CHARACTER SET `utf8mb4` COLLATE `utf8mb4_unicode_ci`');
```

Laravel

Create database that specified in configuration file (`config/database.php`) (using default connection (`DB_CONNECTION`))

```bash
php artisan migrate:install
```

With specific connection

```bash
php artisan migrate:install --database="pgsql"
```

Using migration

```php
DB::statement('CREATE DATABASE IF NOT EXISTS `cookbook` CHARACTER SET `utf8mb4` COLLATE `utf8mb4_unicode_ci`');
```

Using existing connection

```php
Schema::connection('mysql')
    ->getConnection()
    ->getDoctrineSchemaManager()
    ->createDatabase('cookbook');
```

## Dropping a database

MySQL

```mysql
DROP DATABASE IF EXISTS `cookbook`
```

Phinx (no special syntax)

```php
$this->execute('DROP DATABASE IF EXISTS `cookbook`');
```

Laravel

```php
DB::statement('DROP DATABASE IF EXISTS `cookbook`');
```

Using existing connection

```php
Schema::connection('mysql')
    ->getConnection()
    ->getDoctrineSchemaManager()
    ->dropDatabase('cookbook');
```

## Todo

- [x] Schema
- [ ] Tables
- [ ] Columns
- [ ] Indexes
- [ ] Foreign Keys

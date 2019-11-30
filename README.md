# PHP Database Migrations Cookbook

In this repository, I have collected the most common examples of working with database migrations. For examples, I will use the syntax for writing migrations from the most popular tools for creating them:

* [Phinx](https://phinx.org/) - [Documentation](http://docs.phinx.org/en/latest/) - [Package](https://packagist.org/packages/robmorgan/phinx)
* [Laravel Database Migrations](https://laravel.com/docs/master/migrations)

All examples will be accompanied by examples of native [MySQL](https://www.mysql.com/) queries.

## Table of Contents

* [Schema](#schema)
    * [Creating a scheme](#creating-a-database)
    * [Dropping a database](#dropping-a-database)
* [Tables](#tables)
    * [Creating a table](#creating-a-table)
    * [Renaming a table](#renaming-a-table)
    * [Dropping a table](#dropping-a-table)
* [Columns](#columns)
    * [Adding a column](#adding-a-column)
        * [Adding an INTEGER column](#adding-an-integer-column)

## Schema

### Creating a database

* check if the database is exists
* set charset
* set collation

**MySQL**

```mysql
CREATE DATABASE IF NOT EXISTS `cookbook` DEFAULT CHARACTER SET `utf8mb4` COLLATE `utf8mb4_unicode_ci`
```

**Phinx**

```php
if (!$this->getAdapter()->hasDatabase('cookbook')) {
    $this->createDatabase('cookbook', [
        'charset' => 'utf8mb4',
        'collation' => 'utf8mb4_unicode_ci',
    ]);
}
```

**Laravel**

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
Schema::connection('mysql')
    ->getConnection()
    ->getDoctrineSchemaManager()
    ->createDatabase('cookbook');
```

### Dropping a database

**MySQL**

```mysql
DROP DATABASE IF EXISTS `cookbook`
```

**Phinx**

```php
if ($this->getAdapter()->hasDatabase('cookbook')) {
    $this->dropDatabase('cookbook');
}
```

**Laravel**

```php
Schema::connection('mysql')
    ->getConnection()
    ->getDoctrineSchemaManager()
    ->dropDatabase('cookbook');
```

## Tables

### Creating a table

* check if the table is exists
* add primary column
* specify engine
* set charset
* set collation
* add comment

**MySQL**

```mysql
CREATE TABLE IF NOT EXISTS `posts`
(
    `id` int unsigned auto_increment primary key
)
    ENGINE = InnoDB
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci
```

**Phinx**

```php
use Phinx\Db\Table\Table;

if (!$this->hasTable('posts')) {
    $this->getAdapter()->createTable(
        new Table('posts', [
            'engine' => 'InnoDB',
            'collation' => 'utf8_general_ci',
            'comment' => 'Table short description',
            // Primary key options
            'id' => 'id', // Primary key name
            'signed' => false,
        ])
    );
}
```

**Laravel**

```php
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

if (!Schema::hasTable('posts')) {
    Schema::create('posts', function (Blueprint $table) {
        $table->engine = 'InnoDB';
        $table->charset = 'utf8mb4';
        $table->collation = 'utf8mb4_unicode_ci';

        $table->integerIncrements('id');
    });

    DB::statement("ALTER TABLE `posts` comment 'Table description'");
}
```

### Renaming a table

**MySQL**

```mysql
ALTER TABLE `posts` RENAME `articles`;
```

**Phinx**

```php
$this->getAdapter()->renameTable('posts', 'articles');
```

**Laravel**

```php
use Illuminate\Support\Facades\Schema;

Schema::rename('posts', 'articles');
```

### Dropping a table

**MySQL**

```mysql
DROP TABLE IF EXISTS `articles`
```

**Phinx**

```php
if ($this->hasTable('articles')) {
    $this->table('articles')
        ->drop()
        ->save();
}
```

**Laravel**

```php
use Illuminate\Support\Facades\Schema;

Schema::dropIfExists('articles');
```

## Columns

### Adding a column

### Adding an INTEGER column

**MySQL**

```mysql
ALTER TABLE `posts` ADD COLUMN likes INTEGER
```

**Phinx**

```php
use Phinx\Db\Adapter\MysqlAdapter;
use Phinx\Db\Table\Column;

$this->table('posts')
    ->addColumn(
        (new Column())
            ->setName('likes')
            ->setType(MysqlAdapter::PHINX_TYPE_INTEGER)
    )
    ->update();
```

**Laravel**

```php
Schema::table('posts', function (Blueprint $table) {
    $table->integer('likes');
});
```

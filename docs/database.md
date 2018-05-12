The legato framework makes it easy for you to connect and manipulate database 
content via support for illuminate database package out of the box.

## Connecting to a Database

To establish a connection to the database simple update you `.env` file with the following:

```.env

DB_DRIVER=mysql 
DB_HOST=localhost
DB_NAME=db_name
DB_USERNAME=db_username
DB_PASSWORD=db_password

```

### Querying the Database

Querying the database is super easy, if you already know how to use Laravel Query builder then you are good to go, let consider a basic example, retrieving all records from the users table


```php

<?php

use Legato\Framework\Connection as DB;

$users = DB::table('users')->get();

```

The get method returns an `Illuminate\Support\Collection` containing the results where each result is an instance of the `PHP StdClass object`. You may access each column's value by accessing the column as a property of the object:

```php

<?php

 foreach ($users as $user)
 {
    echo $user->username;
 }

```

**Retrieving A Single Row / Column From A Table**

If you just need to retrieve a single row from the database table, you may use the first method. This method will return a single StdClass object:

```php

<?php

$user = DB::table('users')->where('name', 'John')->first();

echo $user->name;

```

If you don't even need an entire row, you may extract a single value from a record using the value method. This method will return the value of the column directly:

```php

<?php

$email = DB::table('users')->where('name', 'John')->value('email');

```

**Retrieving A List Of Column Values**

If you would like to retrieve a Collection containing the values of a single column, you may use the pluck method. In this example, we'll retrieve a Collection of role titles:

```php

<?php

$titles = DB::table('roles')->pluck('title');

foreach ($titles as $title) {
    echo $title;
}

```

You may also specify a custom key column for the returned Collection:

```php

<?php

$roles = DB::table('roles')->pluck('title', 'name');

foreach ($roles as $name => $title) {
    echo $title;
}

```

**Chunking Results**

If you need to work with thousands of database records, consider using the chunk method. This method retrieves a small chunk of the results at a time and feeds each chunk into a Closure for processing. This method is very useful for writing Artisan commands that process thousands of records. For example, let's work with the entire users table in chunks of 100 records at a time:

```php

<?php

DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    foreach ($users as $user) {
        //
    }
});

```

You may stop further chunks from being processed by returning false from the Closure:

```php

<?php

DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    // Process the records...

    return false;
});

```

**Aggregates**

The query builder also provides a variety of aggregate methods such as count, max, min, avg, and sum. You may call any of these methods after constructing your query:

```php

<?php

$users = DB::table('users')->count();

$price = DB::table('orders')->max('price');

Of course, you may combine these methods with other clauses:

$price = DB::table('orders')
                ->where('finalized', 1)
                ->avg('price');

```

**Determining If Records Exist**

Instead of using the count method to determine if any records exist that match your query's constraints, you may use the exists and doesntExist methods:

```PHP

<?php

return DB::table('orders')->where('finalized', 1)->exists();

return DB::table('orders')->where('finalized', 1)->doesntExist();

```

**Specifying A Select Clause**

Of course, you may not always want to select all columns from a database table. Using the select method, you can specify a custom select clause for the query:

`$users = DB::table('users')->select('name', 'email as user_email')->get();`

The distinct method allows you to force the query to return distinct results:

`$users = DB::table('users')->distinct()->get();`

If you already have a query builder instance and you wish to add a column to its existing select clause, you may use the addSelect method:

`$query = DB::table('users')->select('name');`

`$users = $query->addSelect('age')->get();`

### Inserts

The query builder also provides an insert method for inserting records into the database table. The insert method accepts an array of column names and values:

DB::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);

You may even insert several records into the table with a single call to insert by passing an array of arrays. Each array represents a row to be inserted into the table:

```php

<?php

DB::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
]);

```

**Auto-Incrementing IDs**

If the table has an auto-incrementing id, use the insertGetId method to insert a record and then retrieve the ID:

```php

<?php

$id = DB::table('users')->insertGetId(
    ['email' => 'john@example.com', 'votes' => 0]
);

```

When using PostgreSQL the insertGetId method expects the auto-incrementing column to be named id. If you would like to retrieve the ID from a different "sequence", you may pass the column name as the second parameter to the insertGetId method.

### Updates

Of course, in addition to inserting records into the database, the query builder can also update existing records using the update method. The update method, like the insert method, accepts an array of column and value pairs containing the columns to be updated. You may constrain the update query using where clauses:

```php

<?php

DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);

```

**Updating JSON Columns**

When updating a JSON column, you should use -> syntax to access the appropriate key in the JSON object. This operation is only supported on databases that support JSON columns:

```php

<?php

DB::table('users')
            ->where('id', 1)
            ->update(['options->enabled' => true]);

```

Most of the content of this page is directly copied from Laravel documentation for [Database Query Builder](https://laravel.com/docs/5.6/queries), visit the link to learn more. 

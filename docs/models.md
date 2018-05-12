Legato Model are based solely on Laravel Eloquent ORM.

## Creating a Model

There are two ways to create a model:

1. Console command
1. Manually creating the file

**Console Command**

To create a model using console command simple type: 

`$ php Legato add:model User`

this will create a User class inside `app/models` off course you can also place the class inside a subdirectory like so

`$ php Legato add:model Foldername/User`

this will place the file inside `app/models/Foldername`

the generated class will look similar to the following:

```php

<?php

namespace App\Models;

use Legato\Framework\Fluent;

class User extends Fluent
{
    /**
     * Database fields that can be populated with mass assignment
     *
     * @var array
     */
    protected $fillable = [];
}

```
if you chooe to create your model manually ensure to add 

`use Legato\Framework\Fluent;` and `extend Fluent` class

**Table Names**

Note that we did not tell Eloquent which table to use for our User model. By convention, the "snake case", plural name of the class will be used as the table name unless another name is explicitly specified. So, in this case, Eloquent will assume the User model stores records in the users table. You may specify a custom table by defining a table property on your model:

```php

<?php

namespace App\Models;

use Legato\Framework\Fluent;

class User extends Fluent
{
    protected $table = 'my_users';
}

```

**Primary Keys**

Eloquent will also assume that each table has a primary key column named id. You may define a protected $primaryKey property to override this convention.

In addition, Eloquent assumes that the primary key is an incrementing integer value, which means that by default the primary key will be cast to an int automatically. If you wish to use a non-incrementing or a non-numeric primary key you must set the public $incrementing property on your model to false. If your primary key is not an integer, you should set the protected $keyType property on your model to string.

**Timestamps**

By default, Eloquent expects created_at and updated_at columns to exist on your tables. If you do not wish to have these columns automatically managed by Eloquent, set the $timestamps property on your model to false:

```php

<?php

namespace App\Models;

use Legato\Framework\Fluent;

class User extends Fluent
{
    public $timestamps = false;
}

```
If you need to customize the format of your timestamps, set the $dateFormat property on your model. This property determines how date attributes are stored in the database, as well as their format when the model is serialized to an array or JSON:

```php

<?php

namespace App\Models;

use Legato\Framework\Fluent;

class User extends Fluent
{
    protected $dateFormat = 'U';
}

```

If you need to customize the names of the columns used to store the timestamps, you may set the CREATED_AT and UPDATED_AT constants in your model:

```php

<?php

class User extends Fluent
{
    const CREATED_AT = 'creation_date';
    const UPDATED_AT = 'last_update';
}

```

### Retrieving Models

Once you have created a model and its associated database table, you are ready to start retrieving data from your database. Think of each Eloquent model as a powerful query builder allowing you to fluently query the database table associated with the model. For example:

```php

<?php

use App\Models\User;

$users = User::all();

foreach (users as $user) {
    echo $user->username;
}

```

**Adding Additional Constraints**

The Eloquent all method will return all of the results in the model's table. Since each Eloquent model serves as a query builder, you may also add constraints to queries, and then use the get method to retrieve the results:

```php

<?php

$users = User::where('activated', 1)
               ->orderBy('created_at', 'desc')
               ->take(5)
               ->get();

```

Since Eloquent models are query builders, you should review all of the methods available on the query builder. You may use any of these methods in your Eloquent queries.

Most of the content of this page is directly copied from Laravel documentation for [Eloquent ORM](https://laravel.com/docs/5.6/eloquent#defining-models), visit the link to learn more.


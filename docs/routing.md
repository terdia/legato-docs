Legato Framework router is a simple implementation of AltoRouter use 
for routing HTTP Request. 

## Where to Define Your Application Routes 

You should define your routes inside routes.php file which is located in the routes folder when you install a fresh copy of Legato there will be one route defined like so 

```php

<?php

# routes/routes.php
/**
 * Your application routes go here
 */

use Legato\Framework\Routing\Route;

/**
    * @param target, 
    * @param handler, fully qualified classname and method or Closure 
    * @param name (optional) must be unique for each route, 
 */

Route::get('/', 'App\Controllers\IndexController@show', 'homepage');

Route::get('/user/{id}', function ($id) {
    echo 'Example route using closure '.$id;
});
```

### Route with Closure or Callback

You can also define a route that uses a Closure or callback as the handler like so

```php

<?php

Route::get('/user/[i:id]', function ($id) {
    echo 'Example route using closure '.$id;
});

```

### Route Controller

```php

<?php

Route::post('/contact', 'App\Controllers\ContactController@save');

```

When you define a controller route, you should also specify 
the fully qualified name of a controller and method that 
will be executed for example for the route /contact the 
controller is ContactController while the method to be 
executed is save, `App\Controllers\ContactController@save`

### Defining GET Routes

```php

<?php
/**
    * @param target, 
    * @param handler, fully qualified classname and method or Closure 
    * @param name (optional) must be unique for each route, 
 */

Route::get('/', 'App\Controllers\IndexController@show');

```

### Defining POST Routes

```php

<?php
/**
    * @param target, 
    * @param handler, fully qualified classname and method or Closure 
    * @param name (optional) must be unique for each route, 
 */

Route::post('/contact', 'App\Controllers\ContactController@save', 'contact_use');

```

The Legato Route class supports all major HTTP Verbs (POST, GET, PUT, PATCH, DELETE) for example to create a route with PATCH HTTP verb simple do the following

```php

<?php 
/**
    * @param target, 
    * @param handler, fully qualified classname and method or Closure 
    * @param name (optional) must be unique for each route, 
 */

Route::patch('/profile', 'App\Controllers\ProfileController@update');

```

### Defining Route Group

```php

<?php

/**
    * @param target, 
    * @param handler, fully qualified classname and method or Closure 
    * @param name (optional) must be unique for each route, 
 */
Route::group('/admin', array(
    ['GET', '/post', 'App\Controllers\PostController@show'],
    
    ['POST', '/post', 'App\Controllers\PostController@create'],

    /**
     * using closure within a route group
     */
    ['GET', '/post/delete/[i:id]', function($id) { 
        echo  'post with '. $id. ' deleted';
    }],
));

```

### Defining Route Resource
If you are using Legato 1.1.0 you can use the resource method 
to create RESTFul route

```php
<?php

Route::resource('/profile', 'ProfileController');

```

this will generate the following routes and expected methods:

```

Verb 	   URI 	                     Method 	        Route Name

GET 	   /profile 	             index 	            profile_index
GET 	   /profile/create 	         showCreateForm     profile_create_form
POST 	   /profile 	             save 	            profile_save
GET 	   /profile/[i:id] 	         show 	            profile_display
GET 	   /profile/[i:id]/edit 	 showEditForm 	    profile_edit_form
POST 	   /profile/[i:id] 	         update 	        profile_update
GET 	   /profile/[i:id]/delete 	 delete 	        profile_delete

```

You should then create all the methods in your controller, like so:

```php

<?php

namespace App\Controllers;

class ProfileController extends BaseController
{
    /**
    * display all profiles
    */
    public function index()
    {
        //
    }
    
    /**
    * Display the form to create a 
    */
    public function showCreateForm()
    {
        //
    } 
    
    /**
    * Save the resource  
    */
    public function save()
    {
        //
    }
    
    /**
    * Display a specific resource 
    *   
    * @param $id
     */
    public function show($id)
    {
        //
    }
    
    /**
    * Show the form to edit the resource
    * 
    * @param $id
     */
    public function showEditForm($id)
    {
        
    }
    
    /**
    * Update the specified resource
    * 
    * @param $id
     */
    public function update($id)
    {
        
    }
    
    /**
    * Delete the specified resource
    * 
    * @param $id
     */
    public function delete($id)
    {
        
    }
}

```
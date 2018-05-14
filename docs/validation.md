Legato provides different methods to easily validate data coming into your application.

## Use case (Create User)

Lets consider a simple example, we shall attempt to create a new User, the first 
thing we need to do define the necessary routes

### Define Routes

```php
<?php

use Legato\Framework\Routing\Route;

/**
 * show the form to create a user
 */

Route::get('/user/create', 'App\Controllers\UserController@showForm', 'create_user_form');

/**
 * handle the create user request, this is where we do validation
 */

Route::post('/user/create', 'App\Controllers\UserController@create', 'create_user');

```

### Create a Controller with Validation Logic

next we add the UserController class and the create method

```php
<?php
namespace App\Controllers;

use App\Models\User;
use Legato\Framework\Validator\Validator;

class UserController extends BaseController
{

    public function create()
    {
        $rules = [
            'username' => ['required' => true, 'min' => 6, 'max' => 20, 'alphaNum' => true, 'unique' => 'users'],
            'email' => ['required' => true, 'email' => true],
            'password' => ['required' => true, 'min' => 6],
        ];
        
        /**
             * @param, postData, the data that is under validation
             * @param, rules, set of rules that must be satisfied
             * @param, custom error messages (optional)
         */
        
        $validator = new Validator($this->request->all(), $rules);
        
        if($validator->fail()){
            //handle validation error
            $errorBag = $validator->error()->get(); //mu
        }
        
        //create user 
        User::create($this->request->all());
    }
}
```

You can see that we only had to create a new instance of
`Legato\Framework\Validator\Validator::class` and pass it the requires parameters, which are
 `postData`, `rules`, and optionally a `custom` error message to replace the default.

## Displaying Validation Errors

If the request data does not pass validation, you can easily get all the 
validation errors from the validator instance 

```php
<?php

     if($validator->fail()){
         //handle validation error
         $errorBag = $validator->error()->get();
    
         foreach ($errorBag as $errors) {
             foreach ($errors as $error){
                 echo $error . '<br />';
             }
         }
     }
 
```

### Getting Validation errors for specific fields

If you will wish to get validation errors for each field separately then you can do the following:
 
```php
<?php

 /**
     * Check if a given key exists in error message
     *
     * @param $key
     * @return bool
 */
 
 $exists = $validator->error()->has('username');


 /**
    *  Get all the validation errors for a specific fields
 * 
    *  @param $key   
    *  @return array
 */

$usernameErrors = $validator->error()->get('username');

/**
    * Get the first validation error for a specific fields
 * 
    * @param $key
    * @return mixed   
 */

$firstErrors = $validator->error()->first('username');

```

## Available Validation Rules

Legato v1.0.9 which is th latest version as at the time of this writing currently provide the following validation rules:

### #Unique rule

This rule allows you to check if a given value exists in a specific database
table, specific the unique rule like so: 

```php
<?php

    $rules = [
        'username' => ['unique' => 'users',],
    ];

``` 

where username is a valid column in the users table.

### #Required rule

The required rule is use to specific that a specific field cannot be empty, the example 
below ensure that the user cannot signup unless they provide a username: 

```php
<?php

    $rules = [
        'username' => ['required' => true,],
    ];

``` 

### #alphaNum rule

The alphaNum rule is use to validate alphanumeric: e.g. Terdia07

```php
<?php

    $rules = [
        'username' => ['alphaNum' => true,],
    ];

``` 

**To be continued**





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

The following validation rule are available starting from 
Legato v1.0.9, there is no validation feature for older versions:

### #Unique rule

The `unique` rule allows you to check if a given value exists in a specific database
table, specific the unique rule like so: 

```php
<?php

    $rules = [
        'username' => ['unique' => 'users',],
    ];

``` 

where username is a valid column in the users table.

### #Required rule

The `required` rule is use to specific that a specific field cannot be empty, the example 
below ensure that the user cannot signup unless they provide a username: 

```php
<?php

    $rules = [
        'username' => ['required' => true,],
    ];

``` 

### #AlphaNum rule

The `alphaNum` rule is use to validate alphanumeric, returns 
true if every character in text under validation is either
a letter or a digit: e.g. Terdia07

```php
<?php

    $rules = [
        'username' => ['alphaNum' => true,],
    ];

``` 

### #Alpha rule

The `alpha` rule is use to validate alphabetic, returns 
true if every character in text under validation is a 
letter from the current locale: e.g. Terdia

```php
<?php

    $rules = [
        'username' => ['alpha' => true,],
    ];

``` 

### #Email rule

The `email` rule is use to validate email address:

```php
<?php

    $rules = [
        'username' => ['email' => true,],
    ];

``` 

### #Numeric rule

The `numeric` rule is use to Check for numeric character, returns true 
if every character in the string is a decimal digit:

```php
<?php

    $rules = [
        'username' => ['numeric' => true,],
    ];

``` 

### #Min rule

The `min` rule is validate the minimum length for the given value, 
returns false if length of the value is less than rule:

```php
<?php

    $rules = [
        'username' => ['min' => 6,],
    ];

``` 

### #Max rule

The `max` rule is validate the maximum length for the given value, 
returns false if length of value is greater than rule:

```php
<?php

    $rules = [
        'username' => ['max' => 6,],
    ];

``` 

### #String rule

The `string` rule will allow you validate fields such as fullname that can have multiple words
 e.g. Osayawe Terry, returns false if the value under validation contains number:

```php
<?php

    $rules = [
        'fullname' => ['max' => 6, 'string' => true],
    ];

``` 

### #Float rule

The `float` rule is use to check if the value under validation 
is a float e.g. 67.0, 89 is also valid:

```php
<?php

    $rules = [
        'price' => ['float' => true],
    ];

``` 

### #Mixed rule

The `mixed` rule will allow you valid a sentence, this is suitable for validating
 data that might contain some special character 
 e.g. That's the car, parked over there, it can contain letters, numbers and
 any of the following A-Za-z0-9 .,_~-!@#\&%'\* special characters:

```php
<?php

    $rules = [
        'post_body' => ['mixed' => true,],
    ];

``` 

### #IP rule

The `ip` rule is use to check if the value under validation 
is a valid IP address e.g. 127.0.0.1:

```php
<?php

    $rules = [
        'IP Address' => ['ip' => true],
    ];

``` 

### #Url rule

The `url` rule is use to check if the value under validation 
is a valid URL e.g. http://docs.legatoframework.com:

```php
<?php

    $rules = [
        'Website' => ['url' => true],
    ];

``` 
**More Validation rules will be added from time to time**





The controller is generally responsible for performing a request action.

## Creating a Controller

You can simply create a controller by using the Legato inbuilt 
command line tool or simply create a new file inside the controllers' 
folder which is inside the app folder. If you use the command line 
tool then everything will be set up for automatically.

### Using the command line tool

To create a controller with the command line tool, first, 
open your terminal or command prompt for windows users then change 
directory to the root of your legato project

```
$ cd /project/root/path

then type

$ php Legato add:controller UserController
```

This will create a new class inside `app/controllers` like so, 
`/app/controllers/UserController` in the namespace namespace `App\Controllers`

```php

<?php

namespace App\Controllers;

class UserController extends BaseController
{
    
}
?>

```

if you like to place your controller in a subfolder then do the following

```
$ php Legato add:controller UserFolder/UserController
```
The location will be /app/controllers/UserFolder/UserController 

### BaseControler

All controller should extend BaseController 
(this is already done for you if you created your controller using 
the inbuilt command line tool) in other to have access to Request.

**You can reference request like so**

```php

<?php

$request = $this->request; // contains the entire request object

```

### A more complete example 

```php

<?php

namespace App\Controllers;

class UserController extends BaseController
{
    public function show()
    {
        /**
         * Retrieve value from request, used for both POST and GET request
         */
        $username = $this->request->input('username'); 

        /**
         * Set a username session 
         */
        session()->set('username', $username);

        /**
         * Get the value of the username session
         */
        $sessionUsername = session()->get('username');
        
        view('home.twig', compact($sessionUsername));
    }
}

```

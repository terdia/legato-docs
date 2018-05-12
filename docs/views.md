# Views

Legato framework supports two main templating engine out of the box: 

1. Twig and 
1. Blade.

The default template engine is Twig, of course, you can easily switch to Blade or use raw PHP if you like, to change your template engine update your .env file as follow:

### Default 

```
TEMPLATE_ENGINE=twig
```

### Using Blade

```
TEMPLATE_ENGINE=blade
```

## Creating a View

Typically all views should be created inside ```resources/views``` folder, depending on the templating engine you choose you will then have to create files that correspond to that template engine for example if you choose to use Blade as your template engine then your file extension should end with .blade.php so if you're creating an about us page you might do the following:

### Blade

about.blade.php 

the structure will look like so

```

resources
|---views
|       about.blade.php

```

### Twig

about.twig 

the structure will look like so

```

resources
|---views
|       about.twig

```

## Loading a view

Loading a view is very simple using the global `view` helper function, which accepts two arguments:

1. The name of the view or file to load and 
1. Optional data array 

To load a particular view file you will use the following method:

`view('name');`

Consider the following example showing how to load the about page from within a controller:

```php

<?php

namespace App\Controllers;

class IndexController extends BaseController
{

    public function show()
    {
        view('about.twig');
    }
}

```
when using blade you don't need to specify the extension 

```php

<?php

namespace App\Controllers;

class IndexController extends BaseController
{

    public function show()
    {
        view('about');
    }
}

```

### Twig Global Variables

If you wish to declare variables when using twig simply add it to the file `config/twig.php` for example to declare a global variable called `app_name` we can do the following:

```php

<?php

return [
    'twig_global' => [
        'app_name' => getenv('APP_NAME'),
    ]
];

```

then inside the view, we can reference it like so: 

```twig

{% block title %} {{ app_name }}{% endblock %}

```






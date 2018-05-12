## Installation Requirements:

* PHP 7.1
* Composer

Installing Legato is very simple, first ensure you have the right PHP version and composer installed then in your command prompt run:

`$ composer create-project legato/legato blog`

The above command will create a new Legato project inside a new folder name blog, then: 

`$ cd blog`

and then run 

`$ composer dump-autoload`

## Running Your Project With VirtualHost

The entry point for your Legato framework project is the index.php file 
which located inside the public directory, of course, you're free to 
change this according to your need. If you choose to follow the 
default setup, then you may need to create an entry in /etc/httpd/vhosts (for apache users on Linux) similar to the following:

```
<VirtualHost *:80>

    DocumentRoot /var/www/html/blog/public

    ServerName example.com

</VirtualHost>
```

**Restart Apache**

`$ service httpd restsart`

Navigate to http://example.com, you should be able to view the app.

## Running Your Project with PHP inbuilt server

This is the faster and easier way to start using Legato framework, to run Legato using the inbuilt php server do one of the following: 

**Default**

Open the terminal and switch to your project root directory and then type the command below

`php legato start` 

This will start a server which you can access at: `http://localhost:8000`

if the server fails to start because it could not find the PHP executable, 
then you may pass in the `--path` option to specify the path for your PHP executable 
for example on a linux OS you may type `which php` to determine the path of your php 
executable in most cases it will be `/usr/bin/php` or simply `php`. 

To specify the path to your PHP executable type:

`php legato start --path=/usr/bin/php`

**Specifying a Different Hostname**

if you wish to specify a different hostname you can pass in an 
optional hostname option to the start command like so:

`php legato start --hostname=example.com`

make sure to create the domain entry inside your hosts file like so:

```hosts
#hosts file
127.0.0.1 example.com
```

This will start a php server which you can access at: `http://example.com:8000`

**Specifying a Different Port number**

if you wish to specify a different port you can pass in an 
optional port option to the start command like so:

`php Legato start --port=8009`

this will start a php server which you can access at: `http://localhost:8009`

**Specifying a Different Hostname and Port number**

if you wish to use a different hostname and port altogether then make sure to create the hostname entry inside your hosts file:

```hosts
#hosts file
127.0.0.1 example.com
```

then issue the start command with both options, the order does not matter

`php Legato start --hostname=example.com --port=8009`

**With all options**

`php Legato start --hostname=example.com --port=8009 --path=/usr/bin/php`

this will start a php server which you can access at: `http://example.com:8009`

## Running Your Project Without VirtualHost

if you choose to run your project without VirtualHost, for example, accessing it with http://localhost/blog/public then ensure to update your .htaccess file as follows:

add RewriteBase or uncomment it if already in the .htaccess file

```.htaccess
#.htaccess file
RewriteEngine on

RewriteBase /public

RewriteCond %{REQUEST_FILENAME} !-f

RewriteRule . index.php [L]

```
then also remember to update css and javascript path from `/css/all.css` absolute to `css.all.css` relative.
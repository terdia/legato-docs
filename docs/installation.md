## Installation Requirements:

* PHP 7.1
* Composer

Installing Legato is very simple, first ensure you have the right PHP version and composer installed then in your command prompt run:

`$ composer create-project legato/legato blog`

The above command will create a new Legato project inside a new folder name blog, then: 

`$ cd blog`

and then run 

`$ composer dump-autoload`

## Running Your Project

The entry point for your Legato framework project is the index.php file which located inside the public directory, of course, you're free to change this according to your need. If you choose to follow the default setup, then you may need to create an entry in /etc/httpd/vhosts (for apache users on Linux) similar to the following:

```
<VirtualHost *:80>

    DocumentRoot /var/www/html/blog/public

    ServerName example.com

</VirtualHost>
```

**Restart Apache**

`$ service httpd restsart`

Navigate to http://example.com, you should be able to view the app.

## Running Your Project Without VirtualHost

if you choose to run your project without VirtualHost, for example, accessing it with http://localhost/blog/public then ensure to update your .htaccess file as follows:

add RewriteBase or uncomment it if already in the .htaccess file

```.htaccess

RewriteEngine on

RewriteBase /public

RewriteCond %{REQUEST_FILENAME} !-f

RewriteRule . index.php [L]

```
then also remember to update css and javascript path from `/css/all.css` absolute to `css.all.css` relative.
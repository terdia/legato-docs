The Legato framework provides support for email sending using SMTP (Gmail, Mailgun, etc,) and Mailgun API. The Legato Mail class is built on top of Swiftmailer library.

### SMTP Setup

To send mail using SMTP first update your `.env` file as follows: 

**Driver**

set `MAIL_DRIVER=smtp`

**Credentials**

```

SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your_smtp_username
SMTP_PASSWORD=your_smtp_password

```

### Mailgun Setup

To send mail using Mailgun API first update your `.env` file as follows: 

**Driver**

set `MAIL_DRIVER=mailgun`

**Credentials**

```

MAILGUN_DOMAIN=your_mailgun_domain
MAILGUN_SECRET=your_mailgun_api_key

```

## Sending Mail

To send an email simply do the following:

```php

<?php

use Legato\Framework\Mail\Mail;

 $params = [
            'to' => ['example@test.com' => 'name'], //recipient name is optional
            'from' => ['no-reply@legato.com' => 'name'], //sender name is optional
            'subject' => 'Sending mail from Legato Framework',
            'body' => 'This is the content'
        ];
      
 $result = Mail::send($params);      

```

if your mail driver is set to SMTP `$result` variable will contain an integer which is the number of recipients that successfully sent to. if you're using mailgun as your driver you should access the message property of the `$result` variable to see the response.

### Sending Mail with Template

You can also specify a file that contains the body of the mail, to so that you first have create the file inside `resources/views` folder, for example lets say you want to all your email templates in a folder name emails you can do the following 

create the folder in the right location `resources/views/emails` then create the actual file lets say welcome.php

to send the mail using the file do the following

```php

<?php  

$params = [
     'to' => ['example@test.com' => 'name'],
     'from' => ['no-reply@legato.com' => 'name'],
     'subject' => 'Sending mail from Legato Framework',
     'body' => ['text' => 'This is the test that will be inserted into the view', 'name' => 'Jake Pattern'],
     'view' => 'emails/welcome.php',
 ];

$result = Mail::send($params);

```

$param['body'] should be an array of key/value pairs the key can be accessed inside the view like so: 

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
</head>
<body>
    Dear <?php echo $data['name']?>,
    
    <p>
        <?php echo $data['text'] ?>
    </p>
</body>
</html>

```

`$data['text']` will be 'This is the test that will be inserted into the view' while `$data['name']` will be 'Jake Pattern'

### Sending Mail with Attachment

To add an attachment to your mail simple add a `file` key to the `$params` array like so

```php

<?php

$params = [
     ...,
     'file' => 'path_to_file',
 ];

$result = Mail::send($params);

```

**Legato current supports single file attachment. **

**Setting replyTo Address**

To set a reply to address simple add `replyTo` key to your `$params` array like so

```php

<?php

$params = [
     ...,
     'replyTo' => 'reply@legato.com',
 ];

```

**Adding Carbon Copy (cc) Addresses**

To add cc addresses simple add `cc` key to your `$params` array like so

```php

<?php

$params = [
     ...,
     'cc' => ['cc_1@legato.com' => 'John Doe', 'cc_1@legato.com'], 
 ];

```

**Adding Blind Carbon Copy (bcc) Addresses**

To add bcc addresses simple add `bcc` key to your `$params` array like so

```php

<?php

$params = [
     ...,
     'bcc' => ['bcc_1@legato.com' => 'John Doe', 'bcc_1@legato.com'], 
 ];

```

**Note that when using Mailgun API bcc and cc addresses will be combined with to address and all recipients can see who each others email address**
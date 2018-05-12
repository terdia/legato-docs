Legato console is built on to of Symfony console, providing some useful commands out of the box, see all available commands simply type: 

`php Legato list`

### Generating Commands

To create a new command, use the add:command Legato command. This command will create a new command class in the app/commands directory. 

`php legato add:command MyCommand`

out file: app/commands/MyCommand.php with the following contents

```php

<?php
namespace App\Command;

use Legato\Framework\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class MyCommand extends Command
{
    /**
     * Identifier for the console command
     *
     * @var string
     */
    protected $commandName = 'example:command';

    /**
     * Command description
     *
     * @var string
     */
    protected $description = 'Sample command without argument';

    public function __construct($name = null)
    {
        parent::__construct($name);
    }
    
    /**
     * You command logic
     *
     * @param InputInterface $input
     * @param OutputInterface $output
     * @return void
     */
    public function execute(InputInterface $input, OutputInterface $output)
    {
        $output->write('This is a sample command.');
    }
}

```

### Registering Your Commands

To register your commands, simple add then to the `app/commands/register.php` like so 

```php

<?php

return [
    \App\Command\Example::class,
    \App\Command\MyCommand::class,
];

```
make sure to use the fully qualified class name as shown in the code snippet above i.e. include namespace if any

### Running You Command

To run you command type:

`php legato example:command` 

and the output based on the example here will be "This is a sample command."

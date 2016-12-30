# ConfigKit 

In order to provide a higher flexibility on the way we build templates for our Yii projects, we built this kit. Is 
somehow the new generation of the widely used `YiiBootstrap` but much better structured and less complex that the 
mentioned was.

As we all know, Yii has a very cumbersome array configuration for its bootstrap process. The `main.php` normally 
contains a lot of information in there regarding application settings, components, modules, parameters and many times 
we find ourselves dealing with a huge file with settings that, even though they are ordered by keys, failed to be 
clear due to the amount of lines within. 
 
ConfigKit tries to solve that issue, allowing us to create project templates with a different bootstrap and 
configuration building process. It proposes the following configuration folder structure: 

```
config 
├── codeception [ application name that contains its configuration ]
|   ├── app.php
├── console [ application name ]
|   | 
|   ├── components 
|   ├── params 
|   └──app.php [ app.php contains simple attribute configuration ]
|
└── web [ application name ]
    | 
    ├── components [ main section on configuration ]
    |   ├── cache.php [ the name of the file, is the name of the component ]
    |   ├── db.php  [ the contents of the file, are the settings of the component ]
    |   └── log.php
    ├── params  [ main section on app configuration ]
    |   └── mail.php 
    └── app.php
    
```

## Config File Examples

### Example of app.php

```php
<?php

use SideKit\Config\ConfigKit;

return [

    /*
     * --------------------------------------------------------------------------
     * Application
     * --------------------------------------------------------------------------
     *
     * Base class for all application classes. Here we configure the attributes
     * that do not hold any object configuration such as "components" or
     * "modules". The configuration of those properties are within submodules of
     * the same name.
     */

    'id' => 'application-id',

    'basePath' => ConfigKit::config()->getBasePath(),

    'vendorPath' => ConfigKit::config()->getVendorPath(),

    'runtimePath' => ConfigKit::config()->getRuntimePath(),

    'language' => ConfigKit::env()->get('APP_LANGUAGE'),

    'bootstrap' => ['log'],
];
```

### Example of a component file: db.php

```php
<?php

use SideKit\Config\ConfigKit;

return [

    /*
     * --------------------------------------------------------------------------
     * Connection
     * --------------------------------------------------------------------------
     *
     * Represents a connection to a database via PDO.
     */

    'class' => 'yii\db\Connection',

    'dsn' => ConfigKit::env()->get('DATABASE_DSN'),

    'username' => ConfigKit::env()->get('DATABASE_USER'),

    'password' => ConfigKit::env()->get('DATABASE_PASSWORD'),

    'charset' => ConfigKit::env()->get('DATABASE_CHARSET'),

    'tablePrefix' => ConfigKit::env()->get('DATABASE_TABLE_PREFIX'),
];
```

## Environment Settings Overrides 

We know how important settings for different environments (test, local, stage, prod) are, and the proposed solution is 
as simple as adding a config folder with the same structure as the mentioned previously within the folder that has the 
name of environment. We have never found ourselves having to clone the entire structure, so simply place the files that 
contains the settings you wish to override. 

```php
env
|
└── local [ the environment name ]
    └── web [ the application which settings we need to override ]
        | 
        ├── components
        |   └── db.php  [ the component (filename) and its settings that we wish to override ]
        └── params
            └── mail.php [ the parameters to override ]
        
```

## Bootstrapping 

We believe that application bootstrapping should also be somehow structured as its configuration. That way, all 
processes are much clear and easier to manage and scale. In the project template sample [link]() you can see a working 
sample of `ConfigKit` library + a proposed bootstrapping process. The following is the startup process of a web 
application: 

```php
<?php

/*
 * --------------------------------------------------------------------------
 * Register auto loaders
 * --------------------------------------------------------------------------
 *
 * Add registered class loaders required for our application.
 *
 */

require __DIR__ . '/../bootstrap/autoload.php';

/*
 * --------------------------------------------------------------------------
 * Initialize SideKit library
 * --------------------------------------------------------------------------
 *
 * This step is required *prior* adding the application script.
 *
 */

require __DIR__ . '/../bootstrap/sidekit.php';

/*
 * --------------------------------------------------------------------------
 * Initialize custom aliases
 * --------------------------------------------------------------------------
 *
 * Add custom aliases to the application. Added after sidekit to take
 * advantage of its loaded configuration values
 */

require __DIR__ . '/../bootstrap/aliases.php';

/*
 * --------------------------------------------------------------------------
 * Configure and Go!
 * --------------------------------------------------------------------------
 *
 * Bootstrap the configuration processes and get and Application ready to use.
 * Applying configuration details in a different file allow us to free up
 * unnecessary code on the entry script.
 */

$app = require __DIR__ . '/../bootstrap/web.php';

$app->run();
```


## Clean code
 
We have added some development tools for you to contribute to the library with clean code: 

- PHP mess detector: Takes a given PHP source code base and look for several potential problems within that source.
- PHP code sniffer: Tokenizes PHP, JavaScript and CSS files and detects violations of a defined set of coding standards.
- PHP code fixer: Analyzes some PHP source code and tries to fix coding standards issues.

And you should use them in that order. 

### Using php mess detector

Sample with all options available:

```bash 
 ./vendor/bin/phpmd ./src text codesize,unusedcode,naming,design,controversial,cleancode
```

### Using code sniffer
 
```bash 
 ./vendor/bin/phpcs -s --report=source --standard=PSR2 ./src
```

### Using code fixer

We have added a PHP code fixer to standardize our code. It includes Symfony, PSR2 and some contributors rules. 

```bash 
./vendor/bin/php-cs-fixer fix ./src --config .php_cs
```

## Testing

- TODO

[![2amigOS!](https://s.gravatar.com/avatar/55363394d72945ff7ed312556ec041e0?s=80)](http://www.2amigos.us) 
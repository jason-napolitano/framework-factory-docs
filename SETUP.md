# Initial Setup
> The first step would be for us to create a bootstrap file. This file is where we will initialize our application,  
> and add the service providers containing the services we want the application to load into the container. In this example,
> we will use `/bootstrap/app.php`. After the entrypoint file has been created, we will want to create a new `app` instance
> and configure the application itself.
```php
<?php

// import the library:
// let's first bring in the FrameworkFactory application manager
 use FrameworkFactory\Application;

// create the app instance: 
// the basePath argument is required, and should point to the
// main directory of your project
$app = Application::build(basePath: __DIR__ . '/../'); 

// configure the application: 
// continue reading to see how to manage app configuration
// ...

// adding providers: 
// continue reading to see how to manage service providers
// ...

// fire the app up: 
// this final step will get everything bootstrapped and finalize
// the initialization process
$app->fire();
```

> Once the bootstrap file has been created, the application is almost ready to go. Next we will
> have to create, and add, service providers to the application in order to register them within
> the container.

## Service Providers
> Once we have created our [Service Classes](https://github.com/FrameworkFactoryPHP/docs/blob/main/SERVICES.md) and [Service Providers](https://github.com/FrameworkFactoryPHP/docs/blob/main/PROVIDERS.md), 
> we can add them to our application. This can be done by calling the `withProviders()` method on the `$app` instance. The `withProviders` method accepts an array of service providers that we want to 
> load into our application. 

```php
<?php
use App\Providers\MessageProvider;

$app->withProviders([
    MessageProvider::class
]);
```


### Auto-discovery
> Service Providers can be auto-discovered and do not need be added to the `withProviders()` method, and the `withProviders()` method can be omitted. 
> In order to achieve this, all auto-discoverable providers need to live within the `App\Providers` namespace. Any auto-discoverable providers __must__
> either end with `ServiceProvider`, or `Provider` to be properly discovered - EG: `LoggerServiceProvider` or `LoggerProvider`. Any classes that do not 
> contain either suffix will be ignored by the application bootstrap process and will not be added to the providers list.
> 
> The main application namespace and its corresponding directory can be customized upon the creation of the `$app` instance. 
> It is important to note that the `appDirectory` parameter is _relative_ to the assigned `baseDir` path.
```php
<?php
 use FrameworkFactory\Application;

$app = Application::build(basePath: __DIR__ . '/../', appNamespace: 'App', appDirectory: 'app');
```
> Now any classes within the `App\Providers` directory that end with either `Provider` or `ServiceProvider` will be 
> automatically added and their services will be loaded into to the container.

## Configuration
> Each application instance has the ability to set configurable options. These configuration options are completely dynamic
> and can be assigned and retrieved from the application as long as they all follow the same nomenclature. 

### Setters
> Config values may be set by calling `setOption($value)` from the `$app` instance, where `option` is the camel case name
> of the custom configuration option. The setter function **_must_** be prefixed by `set`.

```php
<?php

$app->setGithubLink('https://github.com/FrameworkFactoryPHP')
$app->setApplicationTitle('My Application')
$app->setVersionNumber('1.0.0')
```

### Getters
> Adversely, the custom config values can be retrieved by calling them as a static method of the `Application` class, using camel case
> formatting and removing the `set` prefix.
```php
<?php

use FrameworkFactory\Application;

Application::githubLink();       // => https://github.com/FrameworkFactoryPHP
Application::applicationTitle(); // => My Application
Application::versionNumber();    // => 1.0.0
```
___
- **See Also: [Service Providers](https://github.com/FrameworkFactoryPHP/docs/blob/main/PROVIDERS.md)**
- **See Also: [Installation](https://github.com/FrameworkFactoryPHP/docs/blob/main/PROVIDERS.md)**
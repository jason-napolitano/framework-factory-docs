# Accessors (Facades)
> An accessor is a static-looking interface that provides convenient access to an object managed by an application's 
> dependency container. Instead of manually creating instances or passing dependencies throughout your codebase, you 
> can call methods directly on the accessor as though they were static methods. Behind the scenes, the accessor resolves 
> the appropriate object from the container and forwards the method call to that object.
> 
> The primary advantage of an accessor is simplicity. It reduces boilerplate code and makes commonly used services easy 
> to access from anywhere in an application. For example, logging, caching, configuration, and event-dispatching services 
> can be accessed through concise, expressive method calls without requiring explicit dependency injection in every class 
> that uses them.

## Creating Accessors
> All accessor classes **_must_** extend the `FrameworkFactory\Application\Accessor` class in order to fully utilize the
> accessor system.
> 
```php
<?php

namespace App\Accessors;

use App\Services\Message as MessageService;
use FrameworkFactory\Application\Accessor;

/**
 * @method static display(string $message) 
 */
class Message extends Accessor
{
    // ...
}
```

### Bindings Resolution
> There are two ways that an Accessor class can gain access to services that have been bound to the container. 

The first method is to use the `ResolvesFor` attribute on the accessor class itself with the value of the ID for the 
service you want to load.
```php
use App\Services\Message as MessageService;
use FrameworkFactory\Application\Accessor;
use FrameworkFactory\Support\Attributes;

#[Attributes\Accessors\ResolvesFor(MessageService::class)]
class Message  extends Accessor
{
    // ...    
}
```

The next method is to assign the `$key` property with the value of the ID for the service you want to load.
```php
use App\Services\Message as MessageService;
use FrameworkFactory\Application\Accessor;

class Message  extends Accessor
{
    protected static string ?$key = MessageService::class;
}
```
> **Important note:** if you attempt to use both methods on one accessor class, the value of the `$key` property will 
> take precedence and therefor supersede what has been loaded into `ResolvesFor`.

# Using Accessors
> Using an accessor class to interact with a dependency that has been  loaded into the container, is quite simple. All
> we need to do, is call the accessor class that we've created, and utilize the method(s) of the dependency it's accessing.

```php
<?php

use App\Accessors\Message;

$message = Message::display('This is my message to the world!');

echo $message;
```

___
- **See Also: [Service Providers](https://github.com/FrameworkFactoryPHP/docs/blob/main/PROVIDERS.md)**
- **See Also: [Lifecycle Hooks](https://github.com/FrameworkFactoryPHP/docs/blob/main/LIFECYCLE_HOOKS.md)**
- **See Also: [Service Classes](https://github.com/FrameworkFactoryPHP/docs/blob/main/SERVICES.md)**
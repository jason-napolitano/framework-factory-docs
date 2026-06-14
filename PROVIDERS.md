# Service Providers

> The backbone of Framework Factory is its versatile [PSR-11](https://www.php-fig.org/psr/psr-11/) compliant [IoC container](https://en.wikipedia.org/wiki/Inversion_of_control).
> IoC containers typically allow services, or dependencies, to be loaded into them in many ways. Framework Factory, for instance, uses [Service Providers](https://github.com/FrameworkFactoryPHP/docs/blob/main/PROVIDERS.md) 
> to accomplish the installation, modification and customization of these services.

> A service provider is a class or module that tells the container how to create, configure, and register services so
> they can be automatically resolved and injected into other parts of an application. Framework Factory uses a specific
> approach to creating service providers, using internal libraries to handle the heavy lifting of certain tasks such as
> the injection of the library, the implementation of the [Context API](https://github.com/FrameworkFactoryPHP/docs/blob/main/CONTEXT_API.md) and executing its [lifecycle hooks](https://github.com/FrameworkFactoryPHP/docs/blob/main/LIFECYCLE_HOOKS.md). 
> 
> For questions about creating and building services, please feel free to review the [Services](https://github.com/FrameworkFactoryPHP/docs/blob/main/SERVICES.md)
> section of the docs before proceeding.

## Building Providers
> Each service provider _**must**_ extend Framework Factory's base `ServiceProvider` class. The `register()` method of 
> that class provides access to the container instance, and all of its features.

```php
<?php

namespace App\Providers;

use FrameworkFactory\Contracts\Providers\ServiceProvider;

class MessageProvider extends ServiceProvider
{
     public function register(): void
     {
        // ...
     }
}
```

## Adding Services
> Below is an example of the code required for your typical service provider class. Each service that is bound to the 
> container is mounted by calling the `bind()` method on the container instance.

```php
<?php

namespace App\Providers;

use FrameworkFactory\Contracts\Providers\ServiceProvider;
use App\Services\Message as MessageService;

class MessageProvider extends ServiceProvider
{
     public function register(): void
     {
        $this->bind(MessageService::class, fn () => new MessageService());
     }
}
```

### Lazy Loading
> Lazy-loading is a programming technique where data, objects, modules, or resources are not loaded into memory until they 
> are actually needed, rather than being loaded upfront. This can improve application startup time, reduce memory usage, 
> lower network traffic, and make large applications feel more responsive because unnecessary work is deferred. 
> 
> Common examples include loading database relationships only when accessed, importing JavaScript modules on demand, or 
> loading images as a user scrolls through a page. However, lazy-loading also has disadvantages: it can introduce small 
> delays when a resource is first requested, make application behavior less predictable, increase code complexity, and 
> sometimes lead to issues such as excessive database queries (for example, the "N+1 query" problem in ORMs) if not implemented 
> carefully. In general, lazy-loading trades immediate resource consumption for deferred execution, which can significantly 
> improve efficiency when many resources may never actually be used.

> Below is an example of the code required for lazy loading. This is done by calling the `singleton()` method on the container 
> instance.  Additionally, we will want to implement the `provides()` method - which returns an array of ID's for singleton 
> services that have been added to the container within that Service Provider class.

```php
<?php

namespace App\Providers;

use FrameworkFactory\Contracts\Providers\ServiceProvider;
use App\Services\Message as MessageService;

class MessageProvider extends ServiceProvider
{
     public function register(): void
     {
        $this->singleton(MessageService::class, fn () => new MessageService());
     }
     
     public function provides(): array
     {
        return [
            MessageService::class
        ];
     }
}

```

## The `boot()` method
> Once a dependency has been loaded into the container using a service provider, and the container has been built, 
> we may want to interact with it prior to calling it from within the application. The `boot()` method of a service 
> container is executed after the container has been built, and all dependencies have been loaded into it which allows 
> them to be interacted with.

```php
<?php

public function boot(): void
{
    // do something ...
}
```

___
- **See Also: [Service Classes](https://github.com/FrameworkFactoryPHP/docs/blob/main/SERVICES.md)**
- **See Also: [Lifecycle Hooks](https://github.com/FrameworkFactoryPHP/docs/blob/main/LIFECYCLE_HOOKS.md)**
- **See Also: [Context API](https://github.com/FrameworkFactoryPHP/docs/blob/main/CONTEXT_API.md)**

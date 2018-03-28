---
title: Framework
visible: true
---

The main purpose of M2IF, if you will not use it indirectly with the M2IF - Simple Console Tool, is to support you, to build your own Magento 2 import service. Therefore, we choosed a Component-Based Architecture. If you want to implement your own custom component to import another Magento 2 entity, e. g. customers, you can and should follow these guidelines.

### Dependency Injection

The M2IF componentens does not care about DI, but they can be tied together by using DI. For the M2IF - Simple Console Tool, that we've implemented as a reference application, we used the [Symonfy DI Container](http://symfony.com/doc/current/components/dependency_injection.html) to compose the application. To make the start as simple as possible, we recommend to also use Symonfy and/or Symfony DI when start writing your own component or application. Therefore each core library provides the necessary Symfony DI configuration files in the directory `symfony/Resources/config/services.xml`. To get an impression on how Symfony DI can be used to composer your application have a look at the reference application. M2IF - Simple Console Tool, which parses the library files on start-up, depending on the used Magento Edition, the apropriate load, initialize and inject the necessary classes.

> For [configuration](#configuration) the symfony service IDs will be used instead of the real class names.

### Component Structure

Assuming you'll start to implement your first component the recommended Componend Structure looks like this

<pre>
composer.json
├── src/
│   ├── Actions/
│   │	└── Processors/
│   ├── Assemblers
│   ├── Callbacks
│   ├── Observers
│   ├── Repositories
│   │	└── CacheWarmers/
│   ├── Services
│   ├── Subjects
│   └── Utils
├── tests/
└── symfony/
	│   ├── DependencyInjection/
	│   ├── Resources/
	│   │	└── config/
    │   │   	└── service.xml
	│   └── Utils/
    │	    └── DependencyInjectionKeys.php
    └── ImportBundle.php
</pre>

### Plug-Ins

Depending on the required functionality, it'll be necessary to implement a plug-in. A plug-in is the highest level when starting a component and in many cases it'll not be necessary to implement a plug-in, as subject or observer level can cover the required needs.

#### When do i need a plug-in?

You should think about implementing a plug-in in either one of these cases

* You want to deal with all artefacts of an import and you want to do something with the files before they'll be processed or after they have been processed, e. g. like the artefact plug-in that compresses all the import artefacts into a ZIP and moves it to a configurable folder 
* You want to load some data from the database or the filesystem, that'll be needed later on in your subjects or observers, during the files will be processed, e. g. like the global data plug-in that loads the available attributes and add's them to the registry
* You need to pre-initialize something, before the main import process starts, e. g. like the cache warmer plug-in that warms the registered repository
* You need to do something after the import has been finished, e. g. like the missing options plug-in that sends a list with missing option values a configurable receiver

> Generally you probably need a plug-in, when you want to do something before or after the main import step, or you need access to all import artefacts at once.

#### Implement a plug-in

The good example is the `TechDivision\Import\Plugins\CacheWarmerPlugin` that is part of the M2IF core. Usually you don't have to write the plug-in from scratch, instead extend the `TechDivision\Import\Plugins\AbstractPlugin` class, that implements the `TechDivision\Import\Plugins\PluginInterface` which **MUST** be implemented by every plug-in. The interface defines the `setPluginConfiguration()` method, which expects the plug-in configuration with the optional parameters, and the `process()` method that'll have to implement the plug-ins main functionality.

The `TechDivision\Import\Plugins\CacheWarmerPlugin` loads all the repositories, that implements the `TechDivision\Import\Repositories\CacheWarmer\CacheWarmerInterface` from the DI container and invokes their `warm()` method that pre-loads the repository data into the cache. For sure, this can be done before the main import process to minimize database queries and system load. 

```php
namespace TechDivision\Import\Plugins;

use TechDivision\Import\Utils\ConfigurationKeys;
use TechDivision\Import\Utils\DependencyInjectionKeys;

class CacheWarmerPlugin extends AbstractPlugin
{

    /**
     * Array with the default cache warmers.
     *
     * @var array
     */
    protected $cacheWarmers = array(
        DependencyInjectionKeys::IMPORT_CACHE_WARMER_EAV_ATTRIBUTE_OPTION_VALUE_REPOSITORY
    );

    /**
     * Process the plugin functionality.
     *
     * @return void
     * @throws \Exception Is thrown, if the plugin can not be processed
     */
    public function process()
    {

        // query whether or not additional cache warmers has been configured
        if ($this->getPluginConfiguration()->hasParam(ConfigurationKeys::CACHE_WARMERS)) {
            // try ot load the cache warmers and merge them with the default ones
            $this->cacheWarmers = array_merge(
                $this->cacheWarmers,
                $this->getPluginConfiguration()->getParam(ConfigurationKeys::CACHE_WARMERS)
            );
        }

        // create the instances and warm the repository caches
        foreach ($this->cacheWarmers as $id) {
            $this->getApplication()->getContainer()->get($id)->warm();
        }
    }
}
```
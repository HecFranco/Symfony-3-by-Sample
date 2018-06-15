
```bash
composer create-project symfony/framework-standard-edition LexikJWTAuthenticationBundle  "3.4.*"
```

*Note*: In addition to the basic installation of [Composer](https://getcomposer.org/), we require the following components, which will be added via the terminal:

* `composer require doctrine/doctrine-cache-bundle`.
* `composer require incenteev/composer-parameter-handler`

> We will use `php bin/console generate:bundle --namespace=BackendBundle --format=yml` to prefix the label of the new Bundle.


| Are you planning on sharing this bundle across multiple applications?           | [no]: no       |
|:--------------------------------------------------------------------------------|----------------|
| Give your bundle a descriptive name, like BlogBundle. Bundle name [BlogBundle]: | BackendBundle  |
| Target Directory [src/]:                                                        | src/           |
| Configuration format (annotation, yml, xml, php) [annotation]:                  | yml            |

-----------------------------------------------------------------------------------------------------

| POSSIBLE SYMFONY 3.4 FAULT |
|----------------------------|

You can give an error when executing it, launching the following message Fatal error: Class **BackendBundle** not found in [C:\wamp64\www\symfony.test\app\AppKernel.php](./app/AppKernel.php) on line 19 (See thread with solution, here).

In this case the error was inside [composer.json](./composer.json),

Inside [C:\wamp64\www\symfony.test\composer.json](./composer.json), we look for:

_[composer.json](./composer.json)_
```diff
...
"autoload": {
        "psr-4": {
--          "AppBundle\\": "src/AppBundle",
++          "": "src/"
        },
        "classmap": [ "app/AppKernel.php", "app/AppCache.php" ]
    }
```

| VERY IMPORTANT | run `composer dump-autoload` to execute the autoload function.       |
|----------------|----------------------------------------------------------------------|


* We run in `php bin/console doctrine:mapping:import BackendBundle yml` console to import the table structure of the base and load them in the bundle **BackendBundle** to their different files **orm.yml**, in this case within [src\BackendBundle\Resources\config\doctrine\](./src/BackendBundle/Resources/config/doctrine/).

* Once the **orm.yml** files are generated, we can update the entities using the console command (first we will launch `php bin/console doctrine:generate:entities`) and then `php bin/console doctrine:generate:entities BackendBundle`.

* If basic updates of the entities were made, we will execute in `php bin/console doctrine:generate:entities BackendBundle` to update the generated entities inside **BackendBundle** (necessary in each data update of the entity), it allows to create the **setters** and **getters**.



-----------------------------------------------------------------------------------------------------

--------------------------------------------------------------

```bash
composer require "lexik/jwt-authentication-bundle"
```

_[app/AppKernel.php](./app/AppKernel.php)_
```diff
<?php

use Symfony\Component\DependencyInjection\ContainerBuilder;
use Symfony\Component\HttpKernel\Kernel;
use Symfony\Component\Config\Loader\LoaderInterface;

class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = [
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\SecurityBundle\SecurityBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
            new Symfony\Bundle\MonologBundle\MonologBundle(),
            new Symfony\Bundle\SwiftmailerBundle\SwiftmailerBundle(),
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
            new Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle(),
++          new Lexik\Bundle\JWTAuthenticationBundle\LexikJWTAuthenticationBundle(),
            new AppBundle\AppBundle(),
        ];
        // ...
```
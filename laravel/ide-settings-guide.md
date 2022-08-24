# IDE Settings Guide

## # PHPStorm

### ## Laravel Plugins

Before you start working with Laravel, make sure that either of the following plugins are installed and enabled:

- [Laravel Idea](https://laravel-idea.com/docs/install) (paid) plugin.
- [Laravel](https://plugins.jetbrains.com/plugin/7532-laravel/) plugin (free)
  and [Laravel IDE helper](https://github.com/barryvdh/laravel-ide-helper) tool. (See how to
  at [PHPStorm documentation](https://www.jetbrains.com/help/phpstorm/laravel.html))

#### ### Install Laravel Idea plugin (Recommended)

See [Laravel Idea documentation](https://laravel-idea.com/docs/install)

#### ### Install Laravel plugin & Laravel IDE helper (Free)

Install and enable _Laravel_ plugin in **_Settings > Languages & Frameworks > PHP > Laravel_**
Install _Laravel IDE helper_:

```shell
composer require --dev barryvdh/laravel-ide-helper
```

Add _Laravel IDE helper_ as a ServiceProvider into the application. In the `config/app.php` file,
add `Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class` under the `providers` element:

```php
return array(
    //...
    'providers' => array(
         // ...
         // Laravel IDE helper
         'Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class',
    ),
    // ...
);
```

Update composer scripts in `composer.json` to enable automation:

```json lines
{
  // ...
  "scripts": {
    "post-update-cmd": [
      // ...
      "@ide-helper"
    ],
    // ...
    "ide-helper": [
      "@php artisan ide-helper:generate",
      "@php artisan ide-helper:meta",
      "@php artisan ide-helper:models -N"
    ]
  }
}
// ...
```

> The Laravel IDE Helper may have to be run (`composer run ide-helper`) after changing or adding services, controllers,
> models and views. 

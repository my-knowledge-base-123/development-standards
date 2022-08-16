# Development Standards - Laravel

## *Table of Contents*

[# Introduction](#-introduction)  
[# Philosophy](#-philosophy)  
[# Rule Priorities](#-rule-priorities)  
[# Design Pattern](#-design-pattern)  
[# Technology Stack](#-technology-stack)  
[# Project Initialisation](#-project-initialisation)  
[# Environment Variables and Configurations](#-environment-variables-and-configurations)  
[# Routers](#-routers)  
[# Eloquent ORM](#-eloquent-orm)  
[# Database](#-database)
[# Controllers](#-controllers)  
[# Services](#-services)  
[# Views](#-views)  
[# Validation](#-validation)  
[# Custom Artisan Commands](#-custom-artisan-commands)  
[# Security](#-security)  
[# Performance Optimisation](#-performance-optimisation)

## # Introduction

This article provides a set of development standards and rules to Laravel developers, for the purpose of:

- Productivity Improvement: Avoid the waste of "decision time".
- Style Uniform: Uniform the coding styles and strategies of the development team.
- Error Reducing: Reduce the error chance made by developers.

> Standards and rules in this article are designed for large and complex Vue.js projects, while some of them are
> considered too strict or redundancy for simple projects. Please make wise choice to avoid over-configuration and
> over-designing.

## # Philosophy

> - DRY (Don't Repeat Yourself): Don't write duplicated logic. If a logic is used multiple times, it should be decoupled
    / refactored.
> - COC (Convention Over Configuration): Give preference to methodologies that recommended by the framework. Don't
    over-configure.
> - KISS (Keep it Simple, Stupid): Write code that is simple, readable, understandable and maintainable. Annotate
    complex code properly. Don't over-design.
> - Chef's Recommendation: Don't DIY if there is an existing solution provided by experienced sponsors.
> - Official Advice: Give preference to solutions that are recommended officially.

## # Rule Priorities

In order to avoid misunderstanding, this article uses different "modal verbs" to indicate the priorities of rules:

> - MUST: There is no other options, follow the rule with no doubt.
> - MUST NOT: Forbidden; DON'T do that at any time.
> - SHOULD: Highly recommended.
> - SHOULD NOT: Highly not recommended, but not mandatory.
> - MAY: Preferred; Not frequently used in this article.

## # Design Pattern

- MVC: Model-View-Controller Framework. You **SHOULD** make each controller as short and readable as possible. If a
  Laravel
  project is too complex to keep controllers tidy, you **SHOULD** select one of following design patterns to refine
  controllers. Your selection **MAY** depend on the complexity of the business logic.
    - Repository Pattern (see
      more: [Laravel Repository Pattern](https://medium.com/@farhadmsyv/laravel-repository-pattern-861c2dd96a32))
    - Service-Repository Patter (see
      more: [Laravel Service-Repository Pattern](https://dev.to/safbalili/implement-crud-with-laravel-service-repository-pattern-1dkl))
- Restful: Build project with standardised HTTP methods (GET, POST, PUT, DELETE, etc) and "resource-based concept".

## # Technology Stack

### ## Version Selection

You **SHOULD** select LTS version of tools and dependencies, or the latest version that has no conflict with other
tools.

You **SHOULD** consider the versions of tools used on production server, so that the project can run on it without
version conflict.

### ## Tools

- Framework: [Laravel](https://laravel.com/)
- Language: [PHP](https://www.php.net/)
- Version Control: [GitHub](https://github.com/)
- Container Platform: Docker powered by [Laravel Sail](https://laravel.com/docs/9.x/sail)
- API Platform: [Postman](https://www.postman.com/)

### ## Recommended Composer Packages

- Permission Management: [Laravel Permission](https://github.com/spatie/laravel-permission)
- Redis Extension: [Predis](https://github.com/predis/predis)
- Excel Imports and Exports: [Laravel Excel](https://laravel-excel.com/)
- Development-level Debugging: [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar)
- Redis Queue Monitor: [Laravel Horizon](https://laravel.com/docs/9.x/horizon)

## # Project Initialisation

See [Laravel Project Initialisation Guide](https://github.com/lifebyte-systems/lifebyte-web-laravel-sample).

- You **MUST** follow the guide in sample to initialise a new Laravel project.
- You **MUST NOT** initialise a project via copy-pasting files from the sample project.
- For ease of collaboration, you **MUST** develop projects in a uniformed docker container. The easiest way to achieve
  it is to use [Laravel Sail](https://laravel.com/docs/9.x/sail).

## # Environment Variables and Configurations

- You **MUST** get/set configurations via `config()` helper function.
- You **MUST** get/set environment via `config()` helper function.
- You **MUST** not use `env()` helper function outside **.config* files under */configs*.

Pros:

- Use `config()` to control configurations, use `env()` to determine different running environment.
- Performance can be improved via `php artisan cache:config`.
- Make code robust and flexible.

## # Routers

### ## Routing Closure

- You **MUST NOT** write routing closure in router files.
- You **MUST** keep router clean and clear, and **MUST NOT** include business logic code.

### ## Resource-based Routes

You **MUST** use Restful (resource-based) routes (
see [resource routing](https://laravel.com/docs/9.x/controllers#actions-handled-by-resource-controller)). A standard
sample of Restful routes:

| Verb      | URI                  | Action  | Route Name     |
|-----------|----------------------|---------|----------------|
| GET       | /photos              | index   | photos.index   |
| GET       | /photos/create       | create  | photos.create  |
| POST      | /photos              | store   | photos.store   |
| GET       | /photos/{photo}      | show    | photos.show    |
| GET       | /photos/{photo}/edit | edit    | photos.edit    |
| PUT/PATCH | /photos/{photo}      | update  | photos.update  |
| DELETE    | /photos/{photo}      | destroy | photos.destroy |

You **SHOULD** use similar naming convention for routes that are not Restful routes. For example:

| Verb      | URI            | Action  | Route Name     |
|-----------|----------------|---------|----------------|
| GET       | /photos/import | index   | photos.index   |
| GET       | /photos/export | create  | photos.create  |

A resource **MUST** be named in plural
You **MAY** use `resource()` function for Restful routes:

```php
<?php

Route::resource('photos', 'PhotoController');
```

If you need to partially declare Restful routes via `resource()` function, you **MUST** use `only` options to list
routes needed. You **MUST NOT** use `except` options to exclude routes unused.

```php
<?php

Route::resource('photos', 'PhotoController', ['only' => ['index', 'show']]);
```

You **MUST** name all the routes via `name()` function. You **MUST** use resource as a prefix:

```php
Route::post('photos/export', 'PhotoController@export')->name('photos.export');
```

You **MUST** use `route()` function to get URL:

```php
<?php

$photo = App\Models\Photo::find(1);

$url = route('photos.show', ['id' => $photo->id]);
```

You **MAY** use `url()` function in some conditions that `route()` is unavailable

```php
<?php

$photo = App\Models\Photo::find(1);

echo url("/photos/{$photo->id}"); 
```

### ## API Routes

You **MUST** group API routes by version. Your *api.php* router file should look like this:

```php
<?php

use App\Http\Controllers\Api\V1\UserController;
use Illuminate\Support\Facades\Route;


Route::prefix('v1')
    ->name('api.v1.')
    ->namespace('App\Http\Controllers\Api\V1')
    ->group(function () {
        Route::resource('users', UserController::class);
        // More routes ...
    });
```

> It is notable that we set `namespace` to locate controllers, see more details in [API Controllers](#-api-controllers).

## # Eloquent ORM

### ## Models

- You **MUST** create a model via Artisan command: `php artisan make:model`
- A model **MUST** be named in singular. e.g. `App\Model\Photo`
- A model class **MUST** be named in singular. e.g. `app/Model/Photo.php`
- A model attribute for a foreign key **MUST** be `resource_id`. e.g. `user_id`, `post_id`

### ## Traits

Some redundant code fragments will make your models "fat", you **SHOULD** use Trait to refine models, for the ease of
readability and maintainability. Traits an alternative approach to inheritance that solves some limitations of single
class inheritance, which PHP uses. This is commonly used to share similar logic across models.

A trait **MUST** be to share similar logic across models (model-relative) rather than business logic. e.g. You **MAY**
declare relationships or local scopes in a trait, while you **MUST NOT** include CRUD operations in a trait.

Let's imagine a couple of models have a Company relationship.

```php
// app/Model/Traits/HasCompany.php

<? php

namespace App\Models\Traits;

use App\Models\Company;
use Illuminate\Database\Eloquent\Concerns\HasRelationships;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

trait HasCompany {

    use HasRelationships;

    public function company() {
        return $this->belongsTo(Company::class);
    }
}
```

Now you can easily share code from the trait, by the keyword `use` supported by PHP.

```php
// app/Model/User.php

<?php

use App\Models\Traits\HasCompany;

class User {
    use HasCompany;
}
```

You **MUST** put trait files under *app/Model/Traits*

## # Database

### ## Tables

- Tables **MUST** be named in plural and `snake_case`. e.g. `photos`, `my_photos`
- Table columns **MUST** be named in `snake_case`. e.g. `view_count`, `is_admin`
- The primary key of each table **MUST** be `id`
- The foreign key of each table **MUST** be `resource_id`. e.g. `user_id`, `post_id`

### ## Migrations

- You **MUST NOT** create/modify/share database using database tools/commands, you **MUST** use migration, and commit it
  to the version control
- You **MUST** use Artisan command to create a migration
- Migrations **MUST** be name in plural. e.g. `2014_10_12_000000_create_users_table.php`
- If you are not in the first development lifecycle (where database has been created and published to the production
  environment), you **SHOULD** modify database tables by creating a new migration file with a meaning for name. e.g.
  create *database/migrations/{timestamp}_add_avatar_and_introduction_to_users_table.php* by command
  `php artisan make:migration add_avatar_and_introduction_to_users_table --table=users`

### ## Seeders

- You **MUST** create seeders via Artisan command
- Seeders **MUST** be name in singular and `PascalCase`. e.g. `UserSeeder`
- You **MUST** seed table data by calling seeders with `call()` function in *database/seeders/BaseSeeder*, rather than directly seeding data in it.
- You **SHOULD** consider to use `WithoutModelEvents` trait to prevent model events from being dispatched

## # Controllers

### ## Naming Convention

// TODO

### ## Keep Tiny & Tidy

// TODO

### ## Repositories

// TODO

### ## API Controllers

## # Services

// TODO

## # Views

## # Validation

## # Custom Artisan Commands

## # Security

### ## DEBUG Mode

// TODO

### ## XXS

// TODO

### ## SQL Injection

// TODO

### ## Massive Assignment

// TODO

### ## CSRF Protection

## # Performance Optimisation

### # Avoid N + 1 Query Problem

### # Caching

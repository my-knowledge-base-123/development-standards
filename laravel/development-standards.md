# Development Standards - Laravel

## *Table of Contents*

[# Introduction](#-introduction)  
[# Concepts](#-concepts)  
[# Rule Priorities](#-rule-priorities)  
[# Design Pattern](#-design-pattern)  
[# Technology Stack](#-technology-stack)  
[# Project Initialisation](#-project-initialisation)  
[# Environment Variables and Configurations](#-environment-variables-and-configurations)  
[# Routers](#-routers)  
[# Eloquent ORM](#-eloquent-orm)  
[# Database](#-database)  
[# Views](#-views)  
[# Controllers](#-controllers)  
[# Request Validation](#-request-validation)  
[# Custom Artisan Commands](#-custom-artisan-commands)  
[# Security](#-security)  
[# Performance Optimisation](#-performance-optimisation)

## # Introduction

This article provides a set of development standards and rules to Laravel developers, for the purpose of:

- Productivity Improvement: Avoid the waste of "decision time".
- Uniform: Uniform the coding styles and developing strategies.
- Error Reducing: Reduce the error chance made by developers.

> Standards and rules in this article are designed for large and complex Laravel projects, while some of them are
> considered too strict or redundancy for simple projects. Please make wise choice to avoid over-configuration and
> over-designing.

## # Concepts

> - DRY (Don't Repeat Yourself): Don't write duplicated logics. If a logic is used multiple times, you should consider
    to decouple/refactor it.
> - COC (Convention Over Configuration): Give preference to methodologies that recommended by the framework. Don't
    over-configure.
> - KISS (Keep it Simple, Stupid):Make your code simple, readable, understandable and maintainable. Annotate complex
    code properly. Don't over-design.
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
- Restful: Build project with standard HTTP methods (GET, POST, PUT, DELETE, etc) and "resource-based concept".

## # Technology Stack

### ## Version Selection

You **SHOULD** select LTS version of tools and dependencies, or the latest version that has no conflict with Laravel and
other tools.

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
  and/or [Clockwork][Clockwork](https://underground.works/clockwork/)
- Redis Queue Monitor: [Laravel Horizon](https://laravel.com/docs/9.x/horizon)

## # Project Initialisation

See [Laravel Project Initialisation Guide](https://github.com/lifebyte-systems/lifebyte-web-laravel-sample).

- You **MUST** follow the guide in sample to initialise each organisational Laravel project.
- You **MUST NOT** initialise a project via copy-pasting the entire sample project.
- For ease of collaboration, you **MUST** develop projects in a uniformed docker container. The easiest way to achieve
  it is to use [Laravel Sail](https://laravel.com/docs/9.x/sail).

## # Environment Variables and Configurations

- You **MUST** get/set configurations via `config()` helper function.
- You **MUST** get/set environment variables via `config()` helper function.
- You **MUST** not use `env()` helper function outside **.config* files under */configs*.

Pros:

- Use `config()` to control configurations, use `env()` to determine different running environment.
- Performance can be improved by configuration loading optimisation.
- Make code robust and flexible.

## # Routers

### ## Routing Closure

- You **MUST NOT** write routing closure in router files.
- You **MUST** keep router clean and clear, and **MUST NOT** include business logic code.

### ## Restful Routes

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

You **SHOULD** follow the same naming convention for routes that are not Restful routes. For example:

| Verb | URI            | Action | Route Name    |
|------|----------------|--------|---------------|
| POST | /photos/import | import | photos.import |
| GET  | /photos/export | export | photos.export |

A resource **MUST** be named in plural
You **MAY** use `resource()` function for Restful routes:

```php
Route::resource('photos', 'PhotoController');
```

If you need to partially declare Restful routes via `resource()` function, you **MUST** use `only` options to list
routes needed. You **MUST NOT** use `except` options to exclude unused routes.

```php
// Good
Route::resource('photos', 'PhotoController', ['only' => ['index', 'show']]);

// Bad
Route::resource('photos', 'PhotoController', ['except' => ['create', 'edit', 'update']]);
```

You **MUST** name all the routes via `name()` function. You **MUST** use resource as a prefix:

```php
Route::get('photos/export', 'PhotoController@export')->name('photos.export');
```

You **MUST** use `route()` function to get URL:

```php
$photo = App\Models\Photo::find(1);

$url = route('photos.show', ['id' => $photo->id]);
```

You **MAY** use `url()` function in some conditions that `route()` is unavailable:

```php
$photo = App\Models\Photo::find(1);

echo url("/photos/{$photo->id}"); 
```

### ## API Routes

You **MUST** group API routes by API version. Your `api.php` route file should be like this:

```php
// routes/api.php

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

> It is notable that we use `namespace()` to navigate API controllers. See API controller
> rules [here](#-api-controllers).

## # Eloquent ORM

### ## Models

- Each model **MUST** be created via `php artisan make:model` command.
- A model **MUST** be named in singular: e.g. `App\Model\Photo`.
- A model class **MUST** be named in singular. e.g. `app/Model/Photo.php`
- A model attribute for a foreign key **MUST** be `resource_id`: e.g. `user_id`, `post_id`

### ## Model Traits

Some redundant code fragments will make your models "fat", you **SHOULD** use `Model Traits` to refine models, for the
ease of readability and maintainability. Model traits are commonly used to share similar logic across models.

Model traits **MUST** be to share similar logic across models (model-relative) rather than business logic. e.g. You **
MAY**
declare relationships or local scopes in a model trait, while you **MUST NOT** include CRUD operations in it.

Let's imagine a couple of models have a Company relationship.

```php
// app/Models/Traits/HasCompany.php

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
// app/Models/User.php

use App\Models\Traits\HasCompany;

class User {
    use HasCompany;
}
```

You **MUST** put trait files under *app/Model/Traits*.

### ## API Resources

#### Resources

- When building APIs, you **MUST** create [API resources](https://laravel.com/docs/9.x/eloquent-resources) for Eloquent
  models.
- You **MUST** use Artisan command to generate resources.
- Resources **MUST** be model-based, named in `PascalCase` and in singular. e.g. `UserResource`.
- Resource files **MUST** be located under `app/Http/Resources`.

#### Resource Collections

- If you would not like to customise
  the [resource collection](https://laravel.com/docs/9.x/eloquent-resources#resource-collections) of a model, you **MUST
  NOT** generate resource collection.
- You **MUST** use Artisan command to generate resource collections.
- Resources **MUST** be model-based, named in `PascalCase` and in singular. e.g. `UserCollection`.
- Resource files **MUST** be located under `app/Http/Resources`.

## # Database

### ## Tables

- Tables **MUST** be named in plural and `snake_case`. e.g. `photos`, `my_photos`.
- Table columns **MUST** be named in `snake_case`. e.g. `view_count`, `is_admin`.
- The primary key of each table **MUST** be `id`.
- The foreign key of each table **MUST** be `resource_id`. e.g. `user_id`, `post_id`.

### ## Migrations

- You **MUST NOT** create/modify/share database using database tools/commands, you **MUST** use migration, and commit it
  to the version control.
- You **MUST** use Artisan command to create a migration.
- Migrations **MUST** be name in plural. e.g. `2014_10_12_000000_create_users_table.php`.
- If you are not in the first development lifecycle (where database has been created and published to the production
  environment), you **SHOULD** modify database tables by creating a new migration file with a meaning for name. e.g.
  create *database/migrations/{timestamp}_add_avatar_and_introduction_to_users_table.php* by command
  `php artisan make:migration add_avatar_and_introduction_to_users_table --table=users`.

### ## Seeders

- You **MUST** create seeders via Artisan command.
- Seeders **MUST** be name in singular and `PascalCase`. e.g. `UserSeeder`.
- You **MUST** seed table data by calling seeders with `call()` function in *database/seeders/DatabaseSeeder*, rather
  than
  directly seeding data in it.
- You **SHOULD** consider to use `WithoutModelEvents` trait to prevent model events from being dispatched.

## # Views

### ## Blade Templates

- If you would not like to separate the front-end from the back-end, you **MUST** firstly consider
  to use [Blade Templates](https://laravel.com/docs/9.x/blade).
- You **MUST** keep your `/view` path clean and clear.
    - You **MUST** categorise your view files, and you **MUST NOT** put any view file directly under `/view`.
    - View files **MUST** be named in 'snake_case'. e.g. `main_content.blade.php`.
    - You **MUST** name `subview` files with `_` prefix. e.g. `_upload_form.blade.php`.
    - `Base view` files **MUST** have prefix `_base`, e.g. `_base_logo.blade.php`.
    - `Layouts view` files **MUST** have prefix `_the`, e.g. `_the_header.blade.php`.
    - You **SHOULD** consider to use `template inheritance` and use `/layouts/the_app.blade.php` as the root layout.
    - Resource views filenames **MUST** keep with Restful routes and resource controllers.

| View                    | Page Content | Route Name    | Controller             |
|-------------------------|--------------|---------------|------------------------|
| `users/index.blade.php` | User list    | `users.index` | `UserController@index` |

Your view directory will be like this:

```text
|-- views
    |-- base    // Common elements
        |-- _base_logo.blade.php
        |-- ...
    |-- layouts
        |-- _the_header.blade.php
        |-- _the_footer.blade.php
        |-- the_app.blade.php
        |-- ...
    |-- pages
        |-- welcome.blade.php
        |-- ...
    |-- resources
        |-- users
            |-- index.blade.php
            |-- show.blade.php
            |-- create_and_edit.blade.php   // You may combine "create" and "edit" view if they share a similar page layout
            |-- ...
```

## # Controllers

### ## Naming Convention

- You **MUST** use Restful resource controllers in preference.
- Controllers **MUST** be named in singular and `PascalCase`. e.g. `UserController`.

### ## API Controllers

- You **MUST** group API controllers by version.
- If you use both `API controllers` and `Web controllers`, You **MUST** put `Web controllers` under `Web` directory,
  and `API controllers` under `Api/Vx (x: API version)` directory.

Your `app/Http/Controllers` file will be like this:

```text
|-- Controllers
    |-- Api
        |-- V1
            |-- Controller  // API base controller
            |-- UserController
            |-- ...
    |-- Web
        |-- Controller  // Web base controller
        |-- UserController
        |-- ...
```

### ## Keep Tiny & Tidy

You **MUST** keep your controllers as small and readable as possible.

- Each controller **MUST** extend the base controller under the same path.
- You **SHOULD** make each controller method name sense, you **SHOULD NOT** write annotation for controller methods.
- You **SHOULD** write annotation for complex code fragments to explain "why to do so", or **SHOULD** consider to
  use [Repositories](#-repositories) and/or [Services](#-services).
- You **SHOULD NOT** write private methods in controllers, you **SHOULD** only store public **routing actions** in
  controllers.
- You **MUST NOT** keep unused methods: if a controller method is not called, it should be removed.
- You **SHOULD** wisely use **service layer** and **repository layer** to refine controllers (see more
  at [Repositories](#-repositories) and [Services](#-services)).
- You **SHOULD NOT** use `try-catch` carefully in controller methods to handle error response, as Laravel has built-in
  error.
  handling, which ease this process. Only catch exceptions you expect to be thrown in that block that way you can
  properly log unexpected exceptions and fix any other bugs in your code that may have caused those, instead of hiding
  them from yourself.

> [Repository vs Service vs Trait](https://stackoverflow.com/questions/60029955/when-to-use-repository-vs-service-vs-trait-in-laravel)

### ## Repositories

Repository layer is used to decouple resource model operations from the corresponding resource controller.

- If you are using TDD (Test-driven development), you **SHOULD** use repository for the ease of unit testing.
- Repositories **MUST** be resource-based and in `PascalCase`. e.g. `UserRepository`.
- Repository files **MUST** be put under *app/Repositories*.
- You **MUST** create an interface for each repository and implement it in the repository.
- You **SHOULD NOT** include resource name in repository functions. e.g. Good: `findById()`; Bad: `getUserById()`.

> See more at: [Laravel Repository Pattern](https://medium.com/@farhadmsyv/laravel-repository-pattern-861c2dd96a32) and
> [Sample Project](https://github.com/lifebyte-systems/lifebyte-web-laravel-sample)

### ## Services

The `Services` is a layer standing between `Controller` and `Repository`, that helps you to abstract your business logic
from your controller methods. It is helpful when you need to use a logic in multiple places (e.g. different
controllers/front-ends).

See [Laravel Sample](https://github.com/lifebyte-systems/lifebyte-web-laravel-sample), where `UserService` methods are
used in both `Api\V1\UserController` and `Web\UserController`.

- Service files **MUST** be put under path _app/Services_.
- Services **MUST** be named in singular and `PascalCase`. e.g. `UserService`.

## # Request Validation

- You **MUST** use [Form Request Validation](https://laravel.com/docs/9.x/validation#form-request-validation) to
  validate requests.
- All form request classes **MUST** extend the base class `app/Http/Requests/Request.php` and overwrite its methods if
  necessary.
- Form request class name **MUST** be resource-based, singular and in `PascalCase`. e.g. `UserRequest`.

See [Sample](https://github.com/lifebyte-systems/lifebyte-web-laravel-sample/tree/main/src/app/Http/Requests).

## # Custom Artisan Commands

Custom Artisan commands **MUST** contain the namespace of the project. For example:

```shell
// Good
php artisan larave-sample:clear-token
php artisan larave-sample:export-users-to-csv

// Bad
php artisan clear-token
php artisan export-users-to-csv
```

## # Security

### ## DEBUG Mode

You **MUST** set env variable `APP_DEBUG=false` at PRODUCTION environment.

### ## XXS (Cross-Site Scripting)

See [this](https://owasp.org/www-community/attacks/xss/) to know what is XXS.

- By default, You **MUST** use `{{ $content }}` in blade template, as we cannot guarantee data supplied by users is
  secure or un-malicious.
- If you do not want your data to be escaped, you **MAY** use `{!! $content !!}`. Then you **MUST** manually purify the
  content with [HTMLPurifier](https://github.com/mewebstudio/Purifier)

### ## SQL Injection

- You should never allow user input to dictate the `column names` referenced by your queries, including "order by"
  columns.
- If you use `DB::raw()` to build complex SQL queries, you **MUST** use `query bindings` to avoid SQL injection.
- Most query methods in `DB` class receive `$bindings` as the second argument. (See
  in [Laravel documentation](https://laravel.com/docs/9.x/queriest)).

### ## Massive Assignment

- Laravel provides whitelist `$fillable` and blacklist `$guarded` to filter massive assignment. You **SHOULD** use them
properly.
- You **MUST NOT** use both `$fillable` and `$guarded` in a single model, as that makes no sense.

### ## CSRF Protection

- You **MUST** use `DELETE` request method to destroy data.
- You **MUST** use `POST`, `PUT` or `PATCH` to update data.

See more in [Laravel Documentation](https://laravel.com/docs/9.x/csrf).

## # Performance Optimisation

### # Avoid N + 1 Queries

- You **MUST** use `Eager loading` properly when loading relative model data to avoid N + 1 queries.
- You **MUST** optimise the amount of SQL queries of each page before push the project to the production environment.
- You **MAY** use tools: [Clockwork](https://underground.works/clockwork/)
  and/or [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar).

### # Loading Optimisation

When deploying to production, you **MUST** optimise Composer's class autoloader map so Composer can
quickly find the proper file to load for a given class:

```shell
composer install --optimize-autoloader --no-dev
```

When deploying to production, you **MUST** optimise configuration loading, route loading and view loading with command:

```shell
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

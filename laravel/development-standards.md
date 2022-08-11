# Development Standards - Laravel

## *Table of Contents*

[# Introduction](#-introduction)  
[# Philosophy](#-philosophy)  
[# Rule Priorities](#-rule-priorities)  
[# Design Pattern](#-design-pattern)  
[# Technology Stack](#-technology-stack)

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

- MVC: Model-View-Controller Framework. You **SHOULD** make each controller as short and readable as possible. If a Laravel
  project is too complex to keep controllers tidy, you **SHOULD** select one of following design patterns to refine
  controllers. You selection **MAY** depend on the complexity of the business logic.
    - Repository Pattern (see more: [Laravel Repository Pattern](https://medium.com/@farhadmsyv/laravel-repository-pattern-861c2dd96a32))
    - Service-Repository Patter (see more: [Laravel Service-Repository Pattern](https://dev.to/safbalili/implement-crud-with-laravel-service-repository-pattern-1dkl))
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

- Permission: [Laravel Permission](https://github.com/spatie/laravel-permission)
- Redis Client: [Predis](https://github.com/predis/predis)
- Excel Imports and Exports: [Laravel Excel](https://laravel-excel.com/)
- Development-level Debugging: [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar)
- Redis Queue Monitor: [Laravel Horizon](https://laravel.com/docs/9.x/horizon)

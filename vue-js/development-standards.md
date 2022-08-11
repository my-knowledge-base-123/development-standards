# Development Standards - Vue.js

## *Table of Contents*

[# Introduction](#introduction)  
[# Philosophy](#philosophy)  
[# Design Pattern](#design-pattern)  
[# Technology Stack](#technology-stack)  
[# Project Initialisation](#project-initialisation)  
[# Environment & Configurations](#environment--configurations)

<a name="introduction"></a>

## Introduction

This article provides a set of development standards and rules to Vue.js developers, for the purpose of:

- Productivity Improvement: Avoid the waste of "decision time".
- Style Uniform: Uniform the coding styles and strategies of the development team.
- Error Reducing: Reduce the error chance made by developers.

> Standards and rules in this article are designed for large and complex Vue.js projects, while some of them are
> considered too strict or redundancy for simple projects. Please make wise choice to avoid over-configuration and
> over-designing.

<a name="philosophy"></a>

## Philosophy

> - DRY (Don't Repeat Yourself): Don't write duplicated logic. If a logic is used multiple times, it should be decoupled
    / refactored.
> - COC (Convention Over Configuration): Give preference to methodologies that recommended by the framework. Don't
    over-configure.
> - KISS (Keep it Simple, Stupid): Write code that is simple, readable, understandable and maintainable. Annotate
    complex code properly. Don't over-design.
> - Chef's Recommendation: Don't DIY if there is an existing solution provided by experienced sponsors.
> - Official Advice: Give preference to solutions that are recommended officially.

<a name="design-pattern"></a>

## Design Pattern

- Modularisation: Design programs by modules. Group program by service logic or models.
- Restful: Build project with standardised HTTP methods and "resource-based concept".

<a name="rule-priorities"></a>

## Rule Priorities

In order to avoid misunderstanding, this article uses different "modal verbs" to indicate the priorities of rules:

> - MUST: There is no other options, follow the rule with no doubt.
> - MUST NOT: Forbidden; DON'T do that at any time.
> - SHOULD: Highly recommended.
> - SHOULD NOT: Highly not recommended, but not mandatory.
> - MAY: Preferred; Not frequently used in this article.

<a name="technology-stack"></a>

## Technology Stack

### Version Selection

You **SHOULD** select LTS version of tools and dependencies, or the latest version that has no conflict with other
tools.

You **SHOULD** consider the versions of tools used on production server, so that the project can run on it without
version conflict.

<a name="project-initialisation"></a>

## Project Initialisation

See [Vue.js Project Initialisation Guide](https://github.com/lifebyte-systems/lifebyte-web-development-standards/blob/main/vue-js/project-initialisation-guide.md)
.

> You **MUST NOT** initialise a project via copy-paste files from another project.

<a name="environment-configurations"></a>

## Environment & Configurations

### Config Layer

It is always helpful to have different configuration values based on the environment where the application is running.
Here is an example: As we all know, an environment variable can only be set as a string (or boolean). When we need to
set a variable called `VITE_IP_WHITELIST` presenting a list of IP addresses to control the access to some web pages,
then there would be a trouble: we have to set it as a string in .env file, and parse it into an array when we use it.
This obviously waste the time and generates a lot of duplicated code that is hard to maintain.

For this reason, we create a config layer to process environment variables. In this layer, all environment values are
processed, and a utility function `config()` is exposed for usage.

You **MUST** use `config()` to get environment variables and configurations. You **MUST NOT** use `import.meta.env`
anywhere in the project expect to */configs* file.

Pros:

- Clear declaration: `config()` for configurations, `import.meta.env` for determine on different running environments.
- DRY: Process complex environment variables in config layer, avoid duplicated code.
- Make code robust and flexible.

### Naming Convention

- Environment variables in *.env* file **MUST** be named in `SCREAM_SNAKE_CASE` and prefixed with `VITE_`.
- Config file names **MUST** be named in `kebab-case` and end with *.config.ts*.
- Config variables **SHOULD** be named in `snake_case`.
- You **MUST NOT** use abbreviation to name files.

### Sample:

See [Vue.js sample project](https://github.com/lifebyte-systems/lifebyte-web-vue-sample).

<a name="assets"></a>

## Assets

### What's In It

*/asset* path keeps globally shared static data , including images, styling files (e.g. vendor styles,
custom styles), shared documents (e.g. privacy policy PDF document), static data (e.g. country list JSON file), etc.

You **MUST** group assets files by type or by purpose.

A common file structure **SHOULD** be following:

```text
|-- /assets
    |-- /images
        |-- /backgrounds
        |-- /icons
    |-- /styles
        |-- style-1.css
        |-- style-2.css
    |-- /documents
        |-- document-1.pdf
        |-- document-2.xlsx
    |-- /data
        |-- static-data-1.json
        |-- static-data-2.js
```

### Naming Convention

- An assets file **MUST** have a meaningful name that well describes its content.
- The name **MUST** in `kebab-case`.
- You **MUST NOT** use abbreviation in a filename.

<a name="utilities-hooks"></a>

## Hooks

- A hook **MUST** be named in `camelCase` and starts with *use* (e.g. useWindowResize.ts)
- You **SHOULD** create a hook carefully to avoid over-design.
    - If a logic is user only once in a single file, it **MAY** be defined locally rather than create a hook;
    - if there are multiple hooks share some values or refer to a same module, you **MAY** merge them to a single
      hook. (e.g. merge *useFetchProduct*, *useUpdateProduct*, *useDeleteProduct* to *useProduct* with
      functions `fetchProduct()`, `updateProduct()`and `deleteProduct()`)
      <a name="components"></a>

## Components

//TODO

<a name="state-management"></a>

## State Management

//TODO

<a name="router"></a>

## Router

//TODO

<a name="requests-apis"></a>

## Requests & APIs

//TODO

<a name="localisation"></a>

## Localisation

//TODO

## Styling

// TODO

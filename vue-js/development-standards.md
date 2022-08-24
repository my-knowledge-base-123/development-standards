# Development Standards - Vue.js

## *Table of Contents*

[# Introduction](#-introduction)  
[# Philosophy](#-concepts)  
[# Design Pattern](#-design-pattern)  
[# Technology Stack](#-technology-stack)  
[# Project Initialisation](#-project-initialisation)  
[# Environment & Configurations](#-environment--configurations)

## # Introduction

This article provides a set of development standards and rules to Vue.js developers, for the purpose of:

- Productivity Improvement: Avoid the waste of "decision time".
- Style Uniform: Uniform the coding styles and strategies of the development team.
- Error Reducing: Reduce the error chance made by developers.

> Standards and rules in this article are designed for large and complex Vue.js projects, while some of them are
> considered too strict or redundancy for simple projects. Please make wise choice to avoid over-configuration and
> over-designing.

## # Concepts

> - DRY (Don't Repeat Yourself): Don't write duplicated logic. If a logic is used multiple times, it should be decoupled
    / refactored.
> - COC (Convention Over Configuration): Give preference to methodologies that recommended by the framework. Don't
    over-configure.
> - KISS (Keep it Simple, Stupid): Write code that is simple, readable, understandable and maintainable. Annotate
    complex code properly. Don't over-design.
> - Chef's Recommendation: Don't DIY if there is an existing solution provided by experienced sponsors.
> - Official Advice: Give preference to solutions that are recommended officially.

## # Design Pattern

- Modularisation: Design programs by modules. Group program by service logic or models.
- Restful: Build project with standardised HTTP methods and "resource-based concept".

## # Rule Priorities

In order to avoid misunderstanding, this article uses different "modal verbs" to indicate the priorities of rules:

> - MUST: There is no other options, follow the rule with no doubt.
> - MUST NOT: Forbidden; DON'T do that at any time.
> - SHOULD: Highly recommended.
> - SHOULD NOT: Highly not recommended, but not mandatory.
> - MAY: Preferred; Not frequently used in this article.

## # Technology Stack

### Version Selection

You **SHOULD** select LTS version of tools and dependencies, or the latest version that has no conflict with other
tools.

You **SHOULD** consider the versions of tools used on production server, so that the project can run on it without
version conflict.

<a name="project-initialisation"></a>

## # Project Initialisation

See [Vue.js Project Sample](https://github.com/lifebyte-systems/lifebyte-web-vue-sample).

> You **MUST NOT** initialise a project via copy-paste files from another project.

## # Environment & Configurations

### Config Layer

It is always helpful to have different configuration values based on the environment where the application is running.
Here is an example: As we all know, an environment variable can only be set as a string (or boolean). When we need to
set a variable called `VITE_IP_WHITELIST` presenting a list of IP addresses to control the access to some web pages,
then there would be a trouble: we have to set it as a string in .env file, and parse it into an array when we use it.
This obviously waste the time and generates a lot of duplicated code that is hard to maintain.

For this reason, we create a config layer to process environment variables. In this layer, all environment values are
processed accessible globally.

You **MUST** use store management tool ([Pinia](https://pinia.vuejs.org/)) to get environment variables and
configurations. You **MUST NOT** use `import.meta.env` anywhere in the project expect to set configurations.

Pros:

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

`/assets` directory keeps globally shared static data, including images, styling files (e.g. vendor styles, custom
styles), shared documents (e.g. privacy policy PDF document), static data (e.g. country list JSON file), etc.

You **MUST** group assets files by type or by purpose.

A common file structure of assets **SHOULD** be following:

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
- You **SHOULD NOT** use abbreviation for a filename.

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

- Component names **MUST** be in `PascalCase`. e.g. `MyComponent.vue`
- Component names **MUST** be multi-word, expect for root `App` component, and built-in components provided by Vue, such
  as `<transition>` or `<component>`.

    ```text
    // Good
    TodoItem.vue
    
    // Bad
    Todo.vue
    ```

- You **MUST** name base components with a prefix `Base`. e.g. `BaseButton.vue`
- You **MUST** name single-instance component with a prefix `The`. e.g. `TheHeader.vue`
- Child components that are tightly coupled with their parent **MUST** include the parent component name as a prefix.

    ```text
    components/
    |- TodoList.vue
    |- TodoListItem.vue
    |- TodoListItemButton.vue
    ```

- Component names **SHOULD** start with the highest-level (often most general) words and end with descriptive modifying
  words.

    ```text
    components/
    |- SearchButtonClear.vue
    |- SearchButtonRun.vue
    |- SearchInputQuery.vue
    |- SearchInputExcludeGlob.vue
    |- SettingsCheckboxTerms.vue
    |- SettingsCheckboxLaunchOnStartup.vue
    ```

- Components with no content **MUST** be self-closing in single-file components, string templates, and JSX - but never
  in DOM templates.

    ```text
    <!-- In single-file components, string templates, and JSX -->
    <MyComponent/>
    
    <!-- In DOM templates -->
    <my-component></my-component>
    ```

- Component names **MUST** always be `PascalCase` in single-file components and string templates - but `kebab-case` in
  DOM templates.

    ```text
    <!-- In single-file components and string templates -->
    <MyComponent/>
    
    <!-- In DOM templates -->
    <my-component></my-component>
    
    <!-- Or Everywhere -->
    <my-component></my-component>
    ```

### ## SFC (Single-file Component)

- You **MUST** use `Composition API` to build components, while `<script setup>` is an optional.
- You **MUST** declare the component name with `name` attribute.
- You **MUST** define `prop` as detailed as possible (specifying at least prop type).
- You **MUST** use `key` with `v-for`.
- You **MUST NOT** use `v-if` with `v-for`

## State Management

//TODO

## Router

//TODO

## Requests & APIs

//TODO

## Localisation

//TODO

## Styles

For applications, styles in a top-level `App` component and in layout components may be global, but all other components
should always be scoped.

Component libraries, however, should prefer a class-based strategy instead of using the `scoped` attribute.

# Build vue framework

## Updating Node.js

> **Compatibility Note**
>
> Vite requires Node.js version >=12.0.0.
> Check your Node.js version:

```shell
$ node -v
```

Update your Node.js to the latest stable version.

```shell
$ nvm install stable
```

## Scaffolding Project with Vite

### Initialise Vite Project

1. Run `npm init vite`
1. Install `create-vite` if necessary
1. Enter project name: some-name
1. Select a framework: vue
1. Select a variant: vue-ts
1. Install dependencies: Run `npm install`
1. Boot project: Run `npm run dev`

### Config `vite.config.ts`

See more Vite config options at [Vite official website](https://vitejs.dev/config/).

```typescript
import {defineConfig} from 'vite'
import vue from '@vitejs/plugin-vue'

import {resolve} from 'path'

export default defineConfig({
    plugins: [vue()],
    resolve: {
        alias: {
            '@': resolve(__dirname, 'src')
        }
    },
    base: './', // Base path
    server: {
        port: 4000,
        cors: true // Allow cross-origin resource sharing
    }
})
```

If cannot find module 'path' or its corresponding type declarations

```shell
$ npm i @types/node -D
```

Config `tsconfig.json` for TypeScript support

```typescript
{
    "compilerOptions"
:
    {
        // ...
        "baseUrl"
    :
        ".",
            "paths"
    :
        {
            "@/*"
        :
            ["src/*"]
        }
    }
,

    // ...
}
```

## Docker setup

### Initialise docker compose

Create *docker-compose.yml* under project root path

```yaml
version: "3"
services:
  app:
    container_name: your-container-name
    user: "root"
    image: node:16
    working_dir: /var/www/html/app/
    entrypoint: /bin/bash
    environment:
      - NODE_ENV=development
    volumes:
      - .:/var/www/html/app
    ports:
      - "80:80"
    tty: true
```

Config *vite.config.ts*: set **host** option

```typescript
export default defineConfig({
    plugins: [vue()],
    server: {
        host: '0.0.0.0',
        port: 80
    }
})
```

Custom docker compose project name in *.env*

```dotenv
COMPOSE_PROJECT_NAME=project-name
```

> To avoid portal conflict, don't forget to set back-end URL port different from 80.
>
> In Laravel, set ```APP_PORT``` in *.env*.

### Run project in Docker

1. Got to the root path
1. Start docker container: ```$ docker compose up -d```
1. Open container's bash: ```$ docker exec -it [container-name] /bin/bash```
1. Install dependencies: `$ npm install`
1. Run dev server: `$ npm run dev`

## EditorConfig

Create file `.editorconfig`

```editorconfig
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
max_line_length = off
trim_trailing_whitespace = false
```

See more config options at [http://editorconfig.org](http://editorconfig.org).

## Prettier

### Install

```shell
$ npm install --save-dev --save-exact prettier
```

### Config `.prettierrc`

Create config file `.prettierrc` at root path

```yaml
{ "useTabs": false, "tabWidth": 2, "printWidth": 200, "singleQuote": true, "trailingComma": "none", "bracketSpacing": true, "semi": false }

```

See more config options at [Prettier official website](https://prettier.io/docs/en/options.html).
> Set `printWidth` with a large number to avoid ESLint error (prettier/prettier) when it's working with ESLint. This
> error often happens when you wrap a long code (HTML attributes at most of the time).
>
> At Prettier v2.6 (next), a new option `singleAttributePerLine` is added, which may effectively resolve this problem.
> Still waiting for it to be supported by `eslint-plugin-prettier` & `eslint-config-prettier`.

### Enable auto format with Prettier on save

Add Prettier script in `package.json`: Format all files (. means all files)

```json
{
  "scripts": {
    "prettier": "npx prettier --write ."
  }
}
```

> Note: You need to install the plugin `Prettier - Code formatter` if use VSCode editor.

## ESLint

### Install Locally

```shell
$ npm install eslint --save-dev
```

### Initialise ESLint

```shell
$ npx eslint --init
```

> You may have to install `@eslint/create-config` if required, by simply type `y`.

1. How would you like to use ESLint?

- **To check syntax, find problems, and enforce code style**

2. What type of modules does your project use?

- **JavaScript modules (import/export)**

3. Which framework does your project use?

- **Vue.js**

4. Does your project use TypeScript?

- **Yes**

5. Where does your code run?

- **Browser**
- **Node**

6. How would you like to define a style for your project?

- **Use a popular style guide**

7. Which style guide do you want to follow?

- **Airbnb: https://github.com/airbnb/javascript**

8. What format do you want your config file to be in?

- **JavaScript**

9. Would you like to install them now?

- **Yes**

10. Which package manager do you want to use?

- **npm**

> Note: You need to install the plugin `ESLint` if use VSCode editor.

### Add NPM Scripts

Add eslint scripts in `package.json`

```json
{
  "scripts": {
    "lint": "eslint . --ext .js,.jsx,.ts,.tsx,.vue",
    "lint:fix": "eslint . --ext .js,.jsx,.ts,.tsx,.vue --fix"
  }
}
```

### Update `.gitignore`

```gitignore
# ...
# Ignore cache generated by 'eslint --cache' option.
.eslintcache
# ...
```

## Resolve Conflicts between Prettier and ESLint

### Install & Config Plugins

Install plugins:

```shell
$ npm i eslint-plugin-prettier eslint-config-prettier -D
```

Add the plugin in `.eslntrc.js`:

```javascript
module.exports = {
    // ...
    extends: [
        'plugin:vue/vue3-essential',
        'airbnb-base',
        'plugin:prettier/recommended' // Add prettier plugin
    ]
    // ...
}
```

### Resolve Other Unexpected Errors

If you run `npm run lint`, you can still see some ESLint errors. Let's resolve them one by one!

#### `- ESLint: 'vite' should be listed in the project's dependencies, not devDependencies (import/no-extraneous-dependencies)`

<details>
   <summary>See Solution</summary>

Add rule:

```javascript
// .eslintrc.cjs

module.exports = {
    rules: {
        'import/no-extraneous-dependencies': ['error', {devDependencies: true}]
    }
}
```

</details>

#### Does not work well with `<script setup>`

See solutions at [eslint-plugin-vue guide](https://eslint.vuejs.org/user-guide/#faq)

#### Unable to resolve path to Vue modules

Sample error: `ESLint: Unable to resolve path to module '@/components/HelloWorld.vue'. (import/no-unresolved)`

<details>
<summary>See Solution</summary>

Install package `eslint-import-resolver-typescript`:

```shell
$ npm install eslint-import-resolver-typescript
```

Config `.eslintrc.cjs`:

```javascript
module.exports = {
    // ...
    settings: {
        'import/resolver': {
            typescript: {} // this loads <rootdir> / tsconfig.json to eslint
        }
    }
}
```

Config `tsconfig.json` if necessary

</details>

#### file extension missing errors

Sample error: `ESLint: Missing file extension "ts"; for '@/hooks/useSomeHook'; (import/extensions)`

<details>
<summary>See Solution</summary>

Config `.eslintrc.js`

```javascript
module.exports = {
    // ...
    rules: {
        // ...
        'import/extensions': [
            'error',
            'ignorePackages',
            {
                js: 'never',
                jsx: 'never',
                ts: 'never',
                tsx: 'never'
            }
        ]

        // ...
    }
}
```

</details>

# Tech Solutions
## # Handle Eslint Errors 

If you run `npm run lint`, you can still see some ESLint errors. Let's resolve them one by one!

### ## ESLint: 'vite' should be listed in the project's dependencies, not devDependencies (import/no-extraneous-dependencies)`

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

---

### ## Does not work well with `<script setup>`

See solutions at [eslint-plugin-vue guide](https://eslint.vuejs.org/user-guide/#faq)

---

### ## Unable to resolve path to Vue modules

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

---

### ## File extension missing errors

Sample error: `ESLint: Missing file extension "ts"; for '@/hooks/useSomeHook'; (import/extensions)`

<details>
<summary>See Solution</summary>

Config `.eslintrc.cjs`

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

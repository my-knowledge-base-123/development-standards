## VSCode
### Prettier
>Prettier is an opinionated code formatter which ensures one unified code format. It can be used in VS Code by installing it from the VS Code Marketplace.  
#### 1. Install the Prettier extension
To work with Prettier in Visual Studio Code, you’ll need to install the extension. Search for **Prettier - Code Formatter** in the Extensions tab. Click Install once you have located the extension:
![alt text](https://scotch-res.cloudinary.com/image/upload/v1563847465/czyb5upqyrai8ves2snj.png)

#### 2. Enable automatically format on save
Use `Command + ,` on **Mac** or `Control + ,` on **Windows** to open the **Settings** menu. Then search for **Editor: Format on Save** and make sure it is checked.
![alt text](https://scotch-res.cloudinary.com/image/upload/v1563847554/lvhbaywrzn8vxoghrvrs.png)

#### 3. Prettier configuration in Settings
Prettier does a lot of things for you by default, but you can also customize the settings. Open the settings menu as above. Then, search for **Prettier**. This will bring up all of the settings that you can change right there in your editor.
![alt text](https://scotch-res.cloudinary.com/image/upload/v1563847570/vf4742o5rdnxazihcfry.png)

#### 4. Prettier configuration in .prettierrc
We uses a local .prettierrc file instead for a local configuration, it can be used to override the global settings.
Create config file `.prettierrc` at root path
```yaml
{ "useTabs": false, "tabWidth": 2, "printWidth": 200, "singleQuote": true, "trailingComma": "none", "bracketSpacing": true, "semi": false }

```

### ESLint 
#### 1. Install the ESLint extension
To integrate ESLint into Visual Studio Code, you will need to install the ESLint extension for Visual Studio Code. Search for ESLint in the Extensions tab. Click Install once you have located the extension:
![alt text](https://assets.digitalocean.com/articles/linting-and-formatting-with-eslint-in-vs-code/2.png)
Once ESLint is installed in Visual Studio Code, you’ll notice colorful underlining in your app.js file highlighting errors. These markers are color-coded based on severity. If you hover over your underlined code, you will see a message that explains the error to you. In this way, ESLint helps us find and remove code and syntax errors.

### Enable Volar on VSCode 
Volar is the official VSCode extension that provides TypeScript support inside Vue SFCs, along with many other great features.
#### 1. Install the Volar extension
Search for ESLint in the Extensions tab. Click Install once you have located the extension:
![alt text](https://i.vimeocdn.com/video/1434485125-0f12ba06cc8689d9e5d741217adf2343256b15974764b8108e5fd3cc04e300a7-d_1280x720?r=pad)

>**Note:** **Vetur** is the previous official VSCode extension for Vue. If you have Vetur currently installed, make sure to **DISABLE** it in Vue 3 projects.

#### 2. Volar Takeover Mode
>This section only applies for VSCode + Volar.

For each project we are running two TS language service instances: one from Volar, one from VSCode's built-in service. This is a bit inefficient and can lead to performance issues in large projects. Volar provides a feature called "Takeover Mode" to improve performance. In takeover mode, Volar provides support for both Vue and TS files using a single TS language service instance. To enable Takeover Mode, you need to disable VSCode's built-in TS language service in **your project's workspace** only by following these steps:

- In your project workspace, bring up the command palette with `Ctrl + Shift + P` (**macOS**: `Cmd + Shift + P`).
- Type `built` and select "**Extensions: Show Built-in Extensions**".
- Type `typescript` in the extension search box (do not remove `@builtin` prefix).
- Click the little gear icon of "**TypeScript and JavaScript Language Features**", and select "**Disable (Workspace)**".
- Reload the workspace. Takeover mode will be enabled when you open a Vue or TS file.
![alt text](https://vuejs.org/assets/takeover-mode.54f7bbf6.png)

## Eslint

JavaScript 的 Lint 工具，保证代码一致性和避免错误。

作用：

- 在运行代码前就发现语法错误和潜在 bug
- 适合用于制定团队代码规范

其规则分为 3 个等级：

- off
- warn
- error

eslint 早期没有代码格式化功能，只能检测错误，不能自动修改，因此出现了 prettier。

## Prettier

代码格式化工具，用于检测代码中的格式问题

Eslint 偏向于把控项目的代码质量，而 Prettier 更偏向于统一项目的编码风格。

## 配置

### eslint

许多脚手架搭建项目时会提供 eslint 选项，即会自动生成 `.eslintrc.{js, cjs, yaml, json}` 文件，配置好以后需要在 vscode 中安装 eslint 插件才能自动格式化。

生成 eslint 配置文件：

```bash
npm init @eslint/config
```

yarn 和pnpm 管理器也是用这个命令，在中间有一步选择包管理器时选择 yarn 即可。

```bash
? What type of modules does your project use? …
❯ JavaScript modules (import/export)
  CommonJS (require/exports)
```

这一步如果选择 js modules 会生成 js 文件。eslint 配置好以后语法检查就会生效了。试着加上 rules 查看效果。

```js
module.exports = {
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:vue/vue3-essential",
        "plugin:@typescript-eslint/recommended"
    ],
    "overrides": [
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": [
        "vue",
        "@typescript-eslint"
    ],
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    }
}
```

此时发现 `cjs` 文件会报错，之前没有报错的 `.commitlint.config.cjs` 也报错了 [[Blog/Git#^3aazou]]，这是由于 eslint 的语法检查，可以在 env 里加上 `env: 'node'`。
 `commitlint.config.cjs` 也是一样。当然还可以把 js 或 cjs 文件改成 json 格式 (json不支持注释)，比如 `.commitlintrc.json`：

```json
{
	"extends": ["@commitlint/config-conventional"]
}
```

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303211917875.png)

来源：[commitlint/guides-local-setup.md at master · conventional-changelog/commitlint (github.com)](https://github.com/conventional-changelog/commitlint/blob/master/docs/guides-local-setup.md#install-commitlint)

配置好 eslint 后，会进行语法检查，但是不会自动按照规则修改，需要手动修改。因此还要配置 prettier。

### prettier

在项目根目录新建 `.prettierrc.cjs` 文件，写入配置项：

```js
module.exports = {
	printWidth: 100, // 单行代码字符数量不能超过100
	tabWidth: 2,
	useTabs: false, // 不使用制表符缩进，使用空格
	semi: false,    // 不需要分号结束
	singleQuote: true, // 使用单引号
	bracketSpacing: true, // 对象左右都需要空格
}
```

配置好以后需要 reload 一下才能生效。


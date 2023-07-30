## Husky

[Husky - Git hooks (typicode.github.io)](https://typicode.github.io/husky/#/)

Husky 用于管理 git hooks

- hooks：一些时机的回调
- git hooks：git 流程中时机的回调，比如 commit 前调用 pre-commit 这个hook

而 git hoos 需要自己写脚本，不太友好，因此出现了 husky

### commitlint

使用 commitlint 来规范 commit 信息，其要求按一下规则填写 commit 信息：

`type(scope?): subject`

- type 表示 commit 类型
- scope 可选，表示当前 commit 修改的模块范围
- subject 是详细说明

type 可为这些值：

- build
- chore
- ci
- docs
- feat (新功能)
- fix
- perf（性能）
- refactor
- revert
- style
- test

根据官网命令安装好 husky 后进行commitlint配置：[commitlint - Lint commit messages](https://commitlint.js.org/#/)

**总结**

1. 全局或部分安装 commitlint：

```bash
yarn global add @commitlint/cli @commitlint/config-conventional --dev
or
yarn add @commitlint/cli @commitlint/config-conventional -D #不全局安装
or
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

2. 导出配置：

```bash
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

> [!error] 
> 这一步如果报错：
> 
> ```bash
> git commit -m "first commit"
> /Users/pengwei/.config/yarn/global/node_modules/@commitlint/load/node_modules/ts-node/dist/index.js:851
>             return old(m, filename);
>                    ^
> Error [ERR_REQUIRE_ESM]: require() of ES Module /Users/pengwei/Desktop/vue-nio/commitlint.config.js from /Users/pengwei/.config/yarn/global/node_modules/cosmiconfig/dist/loaders.js not supported.
> commitlint.config.js is treated as an ES module file as it is a .js file whose nearest parent package.json contains "type": "module" which declares all .js files in that package scope as ES modules.
> Instead rename commitlint.config.js to end in .cjs, change the requiring code to use dynamic import() which is available in all CommonJS modules, or change "type": "module" to "type": "commonjs" in /Users/pengwei/Desktop/vue-nio/package.json to treat all .js files as CommonJS (using .mjs for all ES modules instead).
> ...
> husky - commit-msg hook exited with code 1 (error)
> ```
> 
> ![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303211640105.png)
> 
> 可以把导出的 `commitlint.config.js` 后缀改成 `cjs`
> 
> 这是因为这个项目被创建为 ES module。可以看下 `package.json`，里面应该有 `type: module` 的配置。
> 
> 于是 `.js` 被默认为使用了 ES module 规范，如果自动生成的配置文件使用了 CommonJS，就会出错。必须写成 `.cjs` 显式地告诉 node.js 它使用了 CommonJS 规范，才不会报错。
> 
> 参考：
> - [typescript - eslint 和 prettier 配置文件不能是 .js 文件，必须是.cjs 文件怎么回事 ？ - SegmentFault 思否](https://segmentfault.com/q/1010000042298464)

^3aazou

3. 在项目内安装 husky：

```bash
npx husky-init && npm install # npm

npx husky-init && yarn # Yarn 1

yarn dlx husky-init --yarn2 && yarn # Yarn 2+

pnpm dlx husky-init && pnpm install # pnpm
```

4. 添加 commitlint hook：

```bash
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

配置好后，如果提交不规范，则会报错：

```bash
git commit -m "first commit"
⧗   input: first commit
✖   subject may not be empty [subject-empty]
✖   type may not be empty [type-empty]

✖   found 2 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint

husky - commit-msg hook exited with code 1 (error)
```

### lint-staged

每次提交都检测所有代码并不是一个好的决定，比如你只修改了文件 A 结果文件 B 报错了，但是文件 B 并不是你负责的模块，改还是不改？

我们可以通过 lint-staged 只对暂存区的代码进行检验。
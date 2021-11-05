# 前端编码规范

**统一的编码规范对团队项目的长远维护起着至关重要的作用. 一致性的代码规范可以增强团队开发协作效率、提高代码质量、减少遗留系统维护的负担**

---

## HTML & Javascript

- Lint 工具
  - [ESLint](https://cn.eslint.org/) - 🔥 目前是社区最流行的、通用的 Javascript Lint 工具，Lint 界的 Babel。支持定制插件、preset。如果不想折腾可以选择它的一些预定义配置
  - [TSLint](https://github.com/palantir/tslint) - Typescript Lint 工具，已废弃, 推荐使用 ESLint
- 规范
  - [JavaScript Standard Style](https://standardjs.com/readme-zhcn.html#why-should-i-use-javascript-standard-style) - 🔥 零配置的、‘标准’的 Javascript 编码规范. 底层基于 Eslint。
  - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) - Airbnb 的编码规范，业界标杆
- 类型检查
  - `Typescript` - 🔥 Javascript 语言的超集，这是一门‘新’的语言，而不是简单的类型检查器. 不过它也支持原生 Javascript 的类型检查
  - `Flow` - Facebook 出品的类型检查器，语法和 `Typescript` 类似. 个人推荐使用 `Typescript`

## CSS

- Lint 工具
  - [stylelint](https://stylelint.docschina.org/) - 🔥 通用的 CSS 编码检查工具，支持最新的 CSS 语法、CSS-in-js、以及其他类 CSS 语法(如 SCSS、Less). 它也有预定义配置，推荐使用
  - 规范
    - [Airbnb CSS / Sass Styleguide](https://github.com/airbnb/css)
    - [Code Guide](https://codeguide.co/)

## 特定框架风格指南

- [vue-style-guide](https://vue.docschina.org/v2/style-guide/)
- [Airbnb React/JSX Style Guide](https://github.com/airbnb/javascript/tree/master/react)

## 代码格式化

- [Prettier](https://prettier.io/) 🔥 关于代码格式化的东西都可以交给它做，在此基础上配合 eslint 做语义检查

## 业界规范

- [腾讯](https://tgideas.qq.com/doc/index.html)
- [凹凸实验室代码规范](https://guide.aotu.io/index.html)
- [百度 FEX 规范](https://github.com/fex-team/styleguide)
- [网易](http://nec.netease.com/standard)

## Code Review

上述的 Lint 工具和类型检查器, 可以约束代码风格、避免低级的语法错误。但是即使通过上面的 Lint 和类型检查，代码也可能未必是‘好代码’,因为代码的最佳实践是无法通过死的规则去实现的，这时候`Code Review` 的作用就体现出来了，在此阶段我们可以检查：

- 编程原则、设计思想
- 模块耦合程度、代码重复
- 代码健壮性。是否存在内存泄露、是否线程安全、是否有潜在性能问题和异常、错误是否被处理
- 代码的性能和效率
- 是否有没有考虑到的场景

`Code Review` 的好处是显而易见的，可以让项目成员熟悉项目，提高代码质量，提升成员代码水平。

## 实践(使用 husky+lint-staged+eslint 规范前端代码)

### Prettier

- 安装使用

1.  运行 `npm install -D prettier` 即可安装到项目中。
2.  添加 `"lint:prettier": "prettier --write **/*.{html,js,vue}"` 命令，运行 `npm run lint:prettier` 即可手动格式化代码

- 配置修改
  添加 `prettier.config.js` 修改默认配置，添加`.prettierignore`设置忽略文件

### Eslint

- 安装使用

1.  运行`npm install -D eslint` 安装到项目
2.  使用 `eslint --init` 初始化`eslint`配置
3.  添加 `"lint": "eslint --fix **/*.{js,jsx,vue}"` 命令，运行 `npm run lint `即可手动格式化代码

- 配置修改
  添加 `.eslintrc.js` 修改默认配置，添加`.eslintignore`设置忽略文件

- 整合 Prettier
  Prettier 提供了 `eslint-config-prettier` 和 `eslint-plugin-prettier` 两个针对 ESLint 的包，前者配置 ESLint 关闭与 Prettier 冲突的规则，后者让 Prettier 作为一个插件运行在 ESLint 中，通过 ESLint 统一检查修改两部分规则

1. 安装两个包 `npm install -D eslint-config-prettier eslint-plugin-prettier`
2. 在 `.eslintrc.js` 中添加配置 `extends: ["plugin:prettier/recommended"]`

### Stylelint

- 安装使用

1.  运行 `npm install -D stylelint stylelint-config-standard` 即可安装到项目中。
2.  添加 `"lint:css": "stylelint --fix **/*.{css,html,vue}"` 命令，运行 `npm run lint:css` 即可手动格式化代码

- 配置修改
  添加 `stylelint.config.js` 修改默认配置，添加`.stylelintignore`设置忽略文件

- 整合 Prettier
  Prettier 提供了 `stylelint-config-prettier` 和 `eslint-prettier` 两个针对 Stylelint 的包，前者配置 Stylelint 关闭与 Prettier 冲突的规则，后者让 Prettier 作为一个插件运行在 Stylelint 中，通过 Stylelint 统一检查修改两部分规则

1. 安装包 `npm install -D stylelint-config-prettier stylelint-prettier stylelint-config-html postcss postcss-html stylelint-config-rational-order`
2. 在 `.stylelintrc.js `中添加配置

```js
module.exports = {
  root: true,
  // customSyntax: 'postcss-less', //具体解析哪种预处理器
  extends: [
    'stylelint-config-standard',
    'stylelint-config-prettier',
    'stylelint-config-recommended',
    'stylelint-config-rational-order', //css顺序插件
  ],
  ignoreFiles: ['**/*.js', '**/*.jsx', '**/*.tsx', '**/*.ts'],
  overrides: [
    {
      files: ['*.vue', '**/*.vue'],
      extends: ['stylelint-config-recommended', 'stylelint-config-html'],
      rules: {
        'selector-pseudo-class-no-unknown': [
          true,
          {
            ignorePseudoClasses: ['deep', 'global'],
          },
        ],
        'selector-pseudo-element-no-unknown': [
          true,
          {
            ignorePseudoElements: ['v-deep', 'v-global', 'v-slotted'],
          },
        ],
      },
    },
  ],
};
```

### Git 整合

通过`husky`，`link-staged`和`git hook`机制，可以实现提交时进行格式化检测和格式操作，保证代码提交的质量。

> ⚠️ 确保 node 版本大于 12.x

1. 运行 `npm install -D husky lint-staged` 安装`husky`、`lint-staged`
2. 运行 `npx husky install` 初始化`husky`配置目录 `.husky`
3. 运行 `npx husky add .husky/pre-commit "npx lint-staged"` 配置`pre-commit hook`
4. 配置 `lint-staged`，可在 package.json 中配置

```json
"lint-staged": {
    "*.{vue,js,ts,md}": "npm run prettier",
    "*.{vue,js,ts}": "npm run lint",
    "*.{css,vue}": [
      "npm run lint:css"
    ]
  },
```

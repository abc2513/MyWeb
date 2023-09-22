# ESLint模块包

https://eslint.nodejs.cn/

## 1.介绍

使用原因

- 统一团队编码规范（命名
- 统一语法（es不同版本语法
- 减少git不必要的更改记录
- 避免低级错误
- 在编译时检查语法

## 2.安装

1.创建项目

- 初始化项目：`npm init -y` (创建 package.json)

2.ESLint安装

1. 直接在项目中安装eslint包 `npm i eslint -D`
2. 注意安装结果：**node_moduels** 中下载了很多包
   - **.bin/eslint.cmd** 脚本文件，内部通过 **nodejs** 执行 **eslint运行包** 的代码
   - **@eslint包** 用来规范 **eslint配置文件**
   - **eslint开头的包** 是 **eslint运行包**，包含eslint代码

3.生成eslint配置文件

建议使用commonJS，因为webpack默认是这样的

````bash
npx eslint --init
````

4.运行eslint检查

```bash
npx eslint ./src/index.js
```

## 3.概念

### 规则

规则是 ESLint 的核心构建块。 规则验证你的代码是否满足特定期望，以及如果不满足该期望该怎么办。 规则还可以包含特定于该规则的其他配置选项。

例如，[`semi`](https://eslint.nodejs.cn/docs/latest/rules/semi) 规则允许你指定 JavaScript 语句是否应以分号 (`;`) 结尾。 你可以将规则设置为始终需要分号或要求语句从不以分号结尾。

ESLint 包含数百个你可以使用的内置规则。 你还可以创建自定义规则或使用其他人通过 [插件](https://eslint.nodejs.cn/docs/latest/use/core-concepts#plugins) 创建的规则。

有关详细信息，请参阅 [规则](https://eslint.nodejs.cn/docs/latest/rules/)。



### 配置文件

ESLint 配置文件是你在项目中放置 ESLint 配置的地方。 你可以包含内置规则、你希望它们如何执行、具有自定义规则的插件、可共享配置、你希望规则应用到哪些文件等等。

有关详细信息，请参阅 [配置文件](https://eslint.nodejs.cn/docs/latest/use/configure/configuration-files)。



### 可共享的配置

可共享配置是通过 npm 共享的 ESLint 配置。

通常可共享配置用于使用 ESLint 的内置规则来强制执行风格指南。 例如，可共享配置 [eslint-config-airbnb-base](https://www.npmjs.com/package/eslint-config-airbnb-base) 实现了流行的 Airbnb JavaScript 风格指南。

有关详细信息，请参阅 [使用可共享的配置包](https://eslint.nodejs.cn/docs/latest/use/configure/configuration-files#using-a-shareable-configuration-package)。



### 插件

ESLint 插件是一个 npm 模块，可以包含一组 ESLint 规则、配置、处理器和环境。 插件通常包含自定义规则。  插件可用于强制执行风格指南并支持 JavaScript 扩展（如 TypeScript）、库（如 React）和框架（Angular）。

插件的一个流行用例是强制执行框架的最佳实践。 例如，[@angular-eslint/eslint-plugin](https://www.npmjs.com/package/@angular-eslint/eslint-plugin) 包含使用 Angular 框架的最佳实践。

有关详细信息，请参阅 [配置插件](https://eslint.nodejs.cn/docs/latest/use/configure/plugins)。



### 解析器

ESLint 解析器将代码转换为 ESLint 可以评估的抽象语法树。 默认情况下，ESLint 使用内置的 [Espree](https://github.com/eslint/espree) 解析器，它与标准的 JavaScript 运行时和版本兼容。

自定义解析器让 ESLint 解析非标准的 JavaScript 语法。 自定义解析器通常包含在可共享配置或插件中，因此你不必直接使用它们。

例如，[@typescript-eslint/parser](https://www.npmjs.com/package/@typescript-eslint/parser) 是一个包含在 [typescript-eslint](https://github.com/typescript-eslint/typescript-eslint) 项目中的自定义解析器，它可以让 ESLint 解析 TypeScript 代码。



### 自定义处理器

ESLint 处理器从其他类型的文件中提取 JavaScript 代码，然后让 ESLint 对 JavaScript 代码进行检查。 或者，你可以使用处理器在使用 ESLint 解析 JavaScript 代码之前对其进行操作。

例如，[eslint-plugin-markdown](https://github.com/eslint/eslint-plugin-markdown) 包含一个自定义处理器，可让你在 Markdown 代码块内对 JavaScript 代码进行检查。



### 格式化器

ESLint 格式化程序控制检查结果在 CLI 中的外观。

有关详细信息，请参阅 [格式化器](https://eslint.nodejs.cn/docs/latest/use/formatters/)。



### 集成

使 ESLint 如此有用的工具之一是围绕它的集成生态系统。 例如，许多代码编辑器都有 ESLint 扩展，可以在你工作时向你显示文件中代码的 ESLint 结果，这样你就无需使用 ESLint CLI 来查看检查结果。

有关详细信息，请参阅 [集成](https://eslint.nodejs.cn/docs/latest/use/integrations)。



### CLI 和 Node.js API

ESLint CLI 是一个命令行接口，可让你从终端执行检查。 CLI 有多种选项可以传递给它的命令。

ESLint Node.js API 允许你以编程方式从 Node.js 代码中使用 ESLint。 该 API 在开发与 ESLint 相关的插件、集成和其他工具时很有用。

除非你以某种方式扩展 ESLint，否则你应该使用 CLI。

有关详细信息，请参阅 [命令行接口](https://eslint.nodejs.cn/docs/latest/use/command-line-interface) 和 [Node.js API](https://eslint.nodejs.cn/docs/latest/integrate/nodejs-api)。

## 4.配置选项

### env环境

[环境](https://eslint.nodejs.cn/docs/latest/use/configure/language-options#specifying-environments) - 你的脚本旨在在哪些环境中运行。 每个环境都带有一组特定的预定义全局变量。

比如浏览器中的window，在其他环境中不存在。eslint不允许出现未声明的变量，因此会报错。要指定运行环境

```json
env:{
    browser:true,
    commonjs:true,
    es2021:true
}
```

### globals全局变量

[全局变量](https://eslint.nodejs.cn/docs/latest/use/configure/language-options#specifying-globals) - 你的脚本在执行期间访问的其他全局变量。

要在配置文件中配置全局变量，请将 `globals` 配置属性设置为包含为你要使用的每个全局变量命名的键的对象。 对于每个全局变量键，设置相应的值等于 `"writable"` 以允许变量被覆盖或 `"readonly"` 不允许覆盖。 例如：

```json
{
    "globals": {
        "var1": "writable",
        "var2": "readonly"
    }
}
```

### extends继承规则包

选择eslint中的规则

```json
extends:[
    //'eslint:all',
    'eslint:recommended'
]
```



### rules规则

ESLint 自带大量的 [内置规则](https://eslint.nodejs.cn/docs/latest/rules/)，你可以通过插件添加更多的规则。 你可以使用配置注释或配置文件修改你的项目使用的规则。

要更改规则的严重性，请将规则 ID 设置为以下值之一：

- `"off"` 或 `0` - 关闭规则
- `"warn"` 或 `1` - 打开规则作为警告（不影响退出代码）
- `"error"` 或 `2` - 打开规则作为错误（触发时退出代码为 1）

规则通常设置为 `"error"`，以在持 Sequelize 成测试、预提交检查和拉取请求合并期间强制遵守规则，因为这样做会导致 ESLint 以非零退出代码退出。

### plugins插件

ESLint 支持使用第三方插件。 在使用插件之前，你必须使用 npm 安装它。

要在配置文件中配置插件，请使用 `plugins` 键，其中包含插件名称列表。 插件名称中可以省略 `eslint-plugin-` 前缀。

```json
{
    "plugins": [
        "plugin1",
        "eslint-plugin-plugin2"
    ]
}
```

### parser解析器

你可以使用自定义解析器将 JavaScript 代码转换为抽象语法树，供 ESLint 评估。 默认情况下，ESLint 使用 [Espree](https://github.com/eslint/espree) 作为其解析器。 

```json
{
    "parser": "esprima",
    "rules": {
        "semi": "error"
    }
}
```

### ignorePatterns忽略文件

你可以通过指定一个或多个通配符模式将 ESLint 配置为在 linting 时忽略某些文件和目录。 你可以通过以下方式忽略文件：

- 将 `ignorePatterns` 添加到配置文件。
- 创建一个包含忽略模式的专用文件（默认为 `.eslintignore`）。

#### 配置文件中

```json
{
    "ignorePatterns": ["temp.js", "**/vendor/*.js"],
    "rules": {
        //...
    }
}
```

- `ignorePatterns` 中的通配符模式与配置文件所在的目录相关。
- 你不能在 `overrides` 属性下写入 `ignorePatterns` 属性。
- `.eslintignore` 中定义的模式优先于配置文件的 `ignorePatterns` 属性。

#### 专用文件中

你可以通过在项目的根目录中创建一个 `.eslintignore` 文件来告诉 ESLint 忽略特定的文件和目录。 `.eslintignore` 文件是一个纯文本文件，其中每一行都是一个通配符模式，指示应该从 linting 中省略哪些路径。 例如，以下省略了所有 JavaScript 文件：

```text
**/*.js
```

运行 ESLint 时，它会在当前工作目录中查找 `.eslintignore` 文件，然后再确定要检查哪些文件。 如果找到此文件，则在遍历目录时应用这些首选项。 一次只能使用一个 `.eslintignore` 文件，所以不使用当前工作目录以外的 `.eslintignore` 文件。
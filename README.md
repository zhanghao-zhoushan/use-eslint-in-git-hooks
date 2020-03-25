# use-eslint-in-git-hooks

📏 使用 ESLint 来管理你的 git commit 代码规范。

## 了解 [Git 钩子](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)

和其它版本控制系统一样，Git 能在特定的重要动作发生时触发自定义脚本。 有两组这样的钩子：客户端的和服务器端的。 客户端钩子由诸如提交和合并这样的操作所调用，而服务器端钩子作用于诸如接收被推送的提交这样的联网操作。 你可以随心所欲地运用这些钩子。

### pre-commit hook

pre-commit 钩子在键入提交信息前运行。 它用于检查即将提交的快照，例如，检查是否有所遗漏，确保测试运行，以及核查代码。 如果该钩子以非零值退出，Git 将放弃此次提交，不过你可以用 git commit --no-verify 来绕过这个环节。 你可以利用该钩子，来检查代码风格是否一致（运行类似 lint 的程序）、尾随空白字符是否存在（自带的钩子就是这么做的），或新方法的文档是否适当。

Git 初始化的时候会在 .git/hooks 文件中生成默认的 hook 文件，都会以 .sample 为后缀，标识 hooks 未生效。我们可以通过重命名删除 .sample 使 hook 生效，然后就可以直接编写 shell 脚本，然而我不会，我只能通过使用 [pre-commit](https://pre-commit.com/) 这个框架来进行 pre-commit 的代码检测。

## pre-commit

pre-commit 能够帮助我们进行 commit 之前的代码风格检查，如果不符合约定的代码风格就不允许提交，并且会提示响应的错误信息，提示你去修改代码。

### 安装 pro-commit

```bash
// use npm
$ npm install pre-commit --save-dev

// use yarn
$ yarn add pre-commit --dev
```

### package.json 文件

编辑 package.json

```json
{
  "scripts": {
    "lint": "eslint src --ext .js --cache --fix"
  },
  "pre-commit": ["lint"]
}
```

运行 lint 脚本会执行 `eslint src --ext .js --cache --fix`，检测 src 文件夹，文件扩展名是 js 的文件，生成 cache 文件，自动修复问题。

## ESLint

ESLint 可以帮助我们统一团队代码规范，形成统一的代码风格。

详细请看 [eslint 官网](https://eslint.org/)

### 安装

全局安装

```bash
// use npm
$ npm install eslint --save-dev

// use yarn
$ yarn add eslint --dev
```

eslint 初始化。

```bash
$ eslint --init
```

### 使用

使用 eslint 检验文件是否符合规定风格。

```bash
$ eslint yourfile.js
```

### 命令行

- `src` 代码检测地址，可以是文件或者文件夹
- `--ext` 指定 JavaScript 文件扩展名
- `--cache` 生成 cache 文件，仅检查已更改的文件
- `--fix` 自动修复问题

具体配置可以看 [Command Line Interface](https://eslint.org/docs/user-guide/command-line-interface)

## husky && lint-staged

我们也可以通过 husky 来进行 commit 之前的代码风格检测。

husky 可以帮助我们阻止 commit、push 不符合代码风格规范的代码。

[husky](https://github.com/typicode/husky)

### 安装 husky

```bash
// use npm
$ npm install husky --save-dev

// use yarn
$ yarn add husky --dev
```

### package.json

编辑 package.json

```json
{
  "scripts": {
    "lint": "eslint ./ --ext .js --cache --fix"
  },
  "husky": {
    "hooks": {
      "pre-commit": "yarn lint"
    }
  }
}
```

你可以在 hooks 中写入 Git 钩子，仅仅是检查代码风格的话，添加 pre-commit 即可。当代码提交之前，会去执行 yarn lint，如果不符合约定的代码风格就不允许提交，并且会提示响应的错误信息，提示你去修改代码。

### 安装 lint-staged

[lint-staged](https://github.com/okonet/lint-staged)

```bash
// use npm
$ npm install lint-staged --save-dev

// use yarn
$ yarn add lint-staged --dev
```

### package.json

编辑 package.json

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/*.{js,jsx}": ["eslint --cache --fix", "git add"]
  }
}
```

当我们执行 git add 命令后，会将代码修改添加到缓存区，当我们执行 git commit 的时候，lint-staged 只会检查修改文件的代码风格规范。

## 参考

[Run npm scripts in a git pre-commit Hook](https://elijahmanor.com/npm-precommit-scripts/)

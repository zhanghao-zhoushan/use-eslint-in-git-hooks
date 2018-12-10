# use-eslint-in-git-hooks

📏 使用 ESLint 来管理你的 git commit 代码规范

## [Git 钩子](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)

和其它版本控制系统一样，Git 能在特定的重要动作发生时触发自定义脚本。 有两组这样的钩子：客户端的和服务器端的。 客户端钩子由诸如提交和合并这样的操作所调用，而服务器端钩子作用于诸如接收被推送的提交这样的联网操作。 你可以随心所欲地运用这些钩子。

### pre-commit hook

pre-commit 钩子在键入提交信息前运行。 它用于检查即将提交的快照，例如，检查是否有所遗漏，确保测试运行，以及核查代码。 如果该钩子以非零值退出，Git 将放弃此次提交，不过你可以用 git commit --no-verify 来绕过这个环节。 你可以利用该钩子，来检查代码风格是否一致（运行类似 lint 的程序）、尾随空白字符是否存在（自带的钩子就是这么做的），或新方法的文档是否适当。

Git 初始化的时候会在 .git/hooks 文件中生成默认的 hook 文件，以 .sample 后缀，可以通过去掉 .sample 使 hook 生效，然后就可以直接编写 shell 脚本。我们也可以使用 [pre-commit](https://pre-commit.com/)。

## pre-commit

pre-commit 用来做提交前的代码风格检查，如果不符合，不允许提交。

### 安装

```bash
npm install pre-commit --save-dev
```

## ESLint

### 安装和使用

全局安装

```bash
$ npm install -g eslint
```

eslint 初始化

```bash
$ eslint --init
```

使用 eslint 检验文件

```js
$ eslint yourfile.js
```

详细请看 [eslint 官网](https://eslint.org/)

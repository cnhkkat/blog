## vscode 插件

- `TypeScript Importer`。通过`:`提供项目内所有的类型定义来进行补全。选择后还会自动导入该类型。
- `Move TS`。改文件的路径，可以直接修改项目的目录结构，并且更新导入。
- `ErrorLens`。将错误直接显示在对应的代码旁边。

## 在线typescript
[Playground](https://link.juejin.cn/?target=https%3A%2F%2Fwww.typescriptlang.org%2Fzh%2Fplay)

## 执行 ts-node index.ts

- 全局安装ts-node typescript 
- 创建 TypeScript 的项目配置文件： tsconfig.json
- 创建一个 TS 文件：index.ts
- 使用 ts-node 执行

```bash
npm i ts-node typescript -g
tsc --init
ts-node index.ts
```

- ts-node 不支持自动地监听文件变更然后重新执行

## ts-node-dev

```bash
npm i ts-node-dev -g
ts-node-dev --respawn --transpile-only app.ts
```

respawn 选项启用了监听重启的能力，而 transpileOnly 提供了更快的编译速度。
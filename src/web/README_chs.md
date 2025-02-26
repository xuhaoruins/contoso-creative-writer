# React + TypeScript + Vite

# React + TypeScript + Vite

React + TypeScript + Vite

该模板提供了一个最小化的设置，使 React 可以在 Vite 中与 HMR 一起工作，并包含了一些 ESLint 规则。

目前，有两个可用的官方插件：

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) 使用 [Babel](https://babeljs.io/) 实现快速刷新（Fast Refresh）
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) 使用 [SWC](https://swc.rs/) 实现快速刷新（Fast Refresh）

## 扩展 ESLint 配置

如果您正在开发一个生产应用程序，我们建议更新配置以启用类型感知的Lint规则：

- 将顶层的 `parserOptions` 属性配置如下：

```js
export default {
  // 其他规则...
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: ['./tsconfig.json', './tsconfig.node.json'],
    tsconfigRootDir: __dirname,
  },
}
```

- 将 `plugin:@typescript-eslint/recommended` 替换为 `plugin:@typescript-eslint/recommended-type-checked` 或 `plugin:@typescript-eslint/strict-type-checked`  
- 可选地添加 `plugin:@typescript-eslint/stylistic-type-checked`  
- 安装 [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react) 并将 `plugin:react/recommended` 和 `plugin:react/jsx-runtime` 添加到 `extends` 列表中  
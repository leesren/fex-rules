# 编辑器

现阶段全部使用 [Visual Studio Code](https://code.visualstudio.com/Download) 编辑器

## 编辑器配置

开发之前的首要配置，统一格式要求。至少保证当前项目是跟团队是一致的。

`文件 -> 首选项 -> 设置`

```json
{
    "editor.rulers": [80, 100],
    "editor.tabSize": 2,
    "editor.lineNumbers": "on",
    "editor.formatOnSave": true,
    "javascript.validate.enable": false,
    "editor.insertSpaces": false,
    "eslint.autoFixOnSave": true,
    "prettier.useTabs": false,
    "prettier.tabWidth": 2,
    "prettier.singleQuote": true,
    "prettier.printWidth": 80
}
```

### 插件要求

推荐安装插件，其他的插件根据项目自己选择

* [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) 拼写检查 
* [Document This](https://marketplace.visualstudio.com/items?itemName=joelday.docthis) 注释 
* [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) 路径检测
* [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) 代码美化
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 代码检测
* [vscode-icons](https://marketplace.visualstudio.com/items?itemName=robertohuertasm.vscode-icons) 图标文件


### 高效

* 自定义代码模板 `ctrl + shift + p`

* 查找文件 `ctrl + p`

* 把 vscode 加入到环境变量中 , `ctrl + shift + p` -> 输入 `code`, 就可以用命令行启动 vscode

如：` code .` 用vscode 打开当前文件夹 

### 快捷键
1. `ctrl + x`, 剪贴 | 删除整行
2. `ctrl + alt + 下`, 向下复制
3. `ctrl -`, 光标跳转


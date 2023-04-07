# 文档简要说明

## 安装

```shell
pnpm i docsify-cli -g
```

## 本地预览

通过运行 docsify serve 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 http://localhost:3000 。

```shell
docsify serve docs
```

更多命令行工具用法，参考 [docsify-cli](https://docsify.js.org/#/zh-cn/) 文档。


## 文档编写流程

1. 打开 `comps` 文件夹，选择对应文件夹，新建 `md` 文件
2. 编写 `md` 文件内容
3. 在 `comps/_sidebar.md` 中编写导航


### 举例

编写 button

对应的是基本组件

新建 `comps/basic/button.md`

TODO: 编写文档

打开 `comps/_sidebar.md`，在基础下新增导航

```md
* 基本组件
  * [按钮 button](comps/basic/button.md)
```

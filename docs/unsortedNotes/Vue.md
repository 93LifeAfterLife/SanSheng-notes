# vue v-for循环的用法

https://www.cnblogs.com/wangyfax/p/10073159.html



## 请求相关

### Referrer Policy

最新版 chrome 为了保证用户访问安全，出了一个新的功能

当你的 **G****enera**l 中没有设置 **Referrer Policy** 的时候，系统会默认设置为 **strict-origin-when-cross-origin**

也就是说，在请求头中，**Referer** 会只有域名，如下，导致 JsonP 类型的请求，后端不作处理返回 403

```xml
Referrer Policy: strict-origin-when-cross-origin
Referer: https://ts.hundsun.com/

当设置 Referrer Policy 为 no-referrer-when-downgrade 的时候， 即
Referrer Policy: no-referrer-when-downgrade
此时，Referrer 字段会显示为全部，如下
Referer: https://ts.hundsun.com/se/portal/SupportPortal.htm
```



## Vue项目优化

### 项目优化策略

1. 生成打包报告
2. 第三方库启用 CDN
3. 组件按需加载
4. 路由懒加载
5. 首页内容定制





Vue 常用依赖

运行依赖:



lodash

echarts

#### NProgress

```javascript
// 导入页面加载进度条
import NProgress from 'nprogress'
import 'nprogress/nprogress.css'

// 设置请求拦截器, 在请求头中绑定 token
axios.interceptors.request.use(config => {
  // 在 request 拦截器中, 展示进度条
  NProgress.start()
  config.headers.Authorization = window.sessionStorage.getItem('token')
  // console.log(config)
  // 语法: 必须return config
  return config
})
// 在 response 拦截器中, 隐藏进度条
axios.interceptors.response.use(config => {
  NProgress.done()
  return config
})
```

#### Vscode Settings. json (手动删除注释)

```
{
  "editor.fontSize": 16,
  "fileheader.LastModifiedBy": "oralinge",
  "fileheader.Author": "oralinge",
  "git.autofetch": true,
  "editor.detectIndentation": false,
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "vetur.validation.template": false, // 标签报错
  // 添加 vue 支持
  "eslint.validate": [
    "vue",
    "html",
    "javascript",
    "typescript"
  ],
  // "prettier.eslintIntegration": true,
  // //  #去掉代码结尾的分号 
  "prettier.semi": false,
  // //  #使用带引号替代双引号 
  // "prettier.singleQuote": true,
  //  #让函数(名)和后面的括号之间加个空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  // 保存缩进空格2
  "editor.tabSize": 2,
  // #这个按用户自身习惯选择 
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  // #让vue中的js按编辑器自带的ts格式进行格式化 
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "vetur.format.defaultFormatterOptions": {
    "prettier": {
      "semi": false
    },
    "js-beautify-html": {
      "wrap_attributes": "auto"
      // #vue组件中html代码格式化样式
      // - auto: 仅在超出行长度时才对属性进行换行。
      // - force: 对除第一个属性外的其他每个属性进行换行。
      // - force-aligned: 对除第一个属性外的其他每个属性进行换行，并保持对齐。
      // - force-expand-multiline: 对每个属性进行换行。
      // - aligned-multiple: 当超出折行长度时，将属性进行垂直对齐。
    }
  },
  // 格式化stylus, 需安装Manta's Stylus Supremacy插件
  // "stylusSupremacy.insertColons": false, // 是否插入冒号
  // "stylusSupremacy.insertSemicolons": false, // 是否插入分好
  // "stylusSupremacy.insertBraces": false, // 是否插入大括号
  // "stylusSupremacy.insertNewLineAroundImports": false, // import之后是否换行
  // "stylusSupremacy.insertNewLineAroundBlocks": false,
  "typescript.format.insertSpaceAfterSemicolonInForStatements": false,
  "workbench.colorTheme": "Monokai Dimmed",
  "window.zoomLevel": 0,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSaveDelay": 100 // 两个选择器中是否换行
}
```



开发依赖:

babel-plugin-transform-remove-console

babel.config.js 文件:

```javascript
// 发布阶段需要用的 babel 插件
const prodPlugins = []
if (process.env.NODE_ENV === 'production') {
  // 表示产品的发布阶段, 使用移除控制台打印插件
  prodPlugins.push('transform-remove-console')
}

module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  plugins: [
    [
      'component',
      {
        libraryName: 'element-ui',
        styleLibraryName: 'theme-chalk'
      }
    ],
    // 判断并使用插件
    ...prodPlugins
  ]
}
```



### 项目上线

#### pm2 管理

服务器端全局安装

```shell
npm i pm2 -g
```

( 若没有脚本权限则进行 ---> ) powershell 管理员运行并使用

```powershell
set-ExecutionPolicy RemoteSigned
```

输入 Y 确认

```powershell
pm2 start app.js --name <ProjectName>
```

pm2 相关操作

```powershell
pm2 ls
pm2 start <id/name>
pm2 restart <id/name>
pm2 delete <id/name>
pm2 stop <id/name>
```





## 项目开发笔记

```
if (res.meta.status !== 200) {
return this.$message.error(res.meta.msg)
 }
// this.$message.success(res.meta.msg)

<!-- 作用域插槽 -->
<template slot-scope="scope">
</template>

<!-- 面包屑导航区域 -->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

<!-- 分页区域 -->
 <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[100, 200, 300, 400]"
      :page-size="100"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400">
    </el-pagination>

// 确认消息提示框
      const confirmRes = await this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).catch(err => err)
      if (confirmRes !== 'confirm') {
        return this.$message.info('取消了删除操作')
      }
<!-- 验证表单 -->
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
  <el-form-item label="活动名称" prop="name">
    <el-input v-model="ruleForm.name"></el-input>
  </el-form-item>
</el-form>

 { required: true, message: '请输入活动名称', trigger: 'blur' },
```


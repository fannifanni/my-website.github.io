---
title: 学习笔记
---

# 学习笔记

## 理解了 HSL 颜色表示法，就能实现 ColorPicker 组件

### [ColorPicker 组件](https://zhuanlan.zhihu.com/p/646595466)

## jest.config.js文件配置


### [jest.testMatch](https://jestjs.io/docs/configuration#testmatch-arraystring)

jest.testRegex接受正则表达式 string，它的功能更强大（但对于您想要实现的目标来说可能有点过头了

[--coverage](https://jestjs.io/docs/cli#--coverageboolean)

别名：--collectCoverage. 指示应收集测试覆盖率信息并在输出中报告。（可选）传递到覆盖配置中设置的选项。


https://github.com/testing-library/react-hooks-testing-library/issues/753

commit 
https://git-scm.com/docs/git-commit

eslint
https://github.com/Stuk/eslint-plugin-header
https://juejin.cn/post/6954900174045085726


jest render component
https://loveky.github.io/2018/06/05/unit-testing-react-component-with-jest/

正则：
https://blog.csdn.net/alike_u/article/details/125492719?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-125492719-blog-79216429.235%5Ev38%5Epc_relevant_sort_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-125492719-blog-79216429.235%5Ev38%5Epc_relevant_sort_base1&utm_relevant_index=5

git stash
https://blog.csdn.net/qq_38709383/article/details/124165567



SourceMapDevToolPlugin
提取source-map-loader现有源映射
以便根据webpack.config.js中的选项指定的所选源映射样式进行处理\


const { SourceMapDevToolPlugin } = require("webpack");
 new SourceMapDevToolPlugin({
        filename: "[file].map"
      }),



source-map提供最好的质量和完整的结果

忽略源映射相关警告
有时，第三方依赖项会导致浏览器检查器中出现与源映射相关的警告。Webpack 允许您按如下方式过滤消息：

const config = {
  stats: {
    ignoreWarnings: { message: /Failed to parse source map/ },
  },
};npm i @edx/frontend-build@12.9.4

 // ignoreWarnings: [/Failed to parse source map/],
     // devtool: "source-map",
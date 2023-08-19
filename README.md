# bem-css

这是一个从`element-ui`组件库中抽取的一组`mixin`，有了这组`mixins`你可以很方便的编写基于`BEM`风格的`scss`代码。

## 快速上手

安装工具集

```bash
$ npm i bem.css
```

使用

```scss
@import "bem.css/scs/mixin.scss";

@include b(a) {
  display: flex;

  @include e(body) {
    display: flex;
    background-color: red;
  }

  @include m(c) {
    display: flex;
    color: pink;

    @include when(active) {
      background-color: blue;
    }
  }
}

/**
  以上代码生成的css为

  .cb-a {
    display: flex;
  }

  .cb__e {
    display: flex;
    background-color: red;
  }

  .cb--c{
    display: flex;
    color: pink;
  }

  .cb--c.is-active {
    background-color: blue;
  }
*/
```

## 配置

在`bem.css`中默认定义了 4 个变量用于控制生成的样式风格，你可以通过覆写它达到修改配置的效果

```scss
// 命名空间
$namespace: "cb";
// 表示子元素的分割符
$element-separator: "__";
// 表示修饰的分割符
$modifier-separator: "--";
// 表示状态的分割符
$state-prefix: "is-";
```

你可以在你需要的地方重新定义这几个变量，从而达到更改配置的效果

## 进阶

在`.vue`文件中使用，我们并不想每次在`style`标签都引入`mixin`，可以通过`scss-loader`的配置实现全局引入。

```js
// webpack.config.js
module.exports = {
  // ...其他配置项
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          "style-loader",
          "css-loader",
          {
            loader: "sass-loader",
            options: {
              // 全局引入
              additionalData: `@import "bem.css/src/mixins.scss";`,
            },
          },
        ],
      },
    ],
  },
  // ...其他配置项
};
```

然后就可以在`.vue`中使用了。如果在`style`标签中您仍然想修改配置，也可以通过覆写变量或者引入一个重新定义变量的`scss`文件即可。

### 全局覆盖

```js
// webpack.config.js
module.exports = {
  // ...其他配置项
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          "style-loader",
          "css-loader",
          {
            loader: "sass-loader",
            options: {
              // 全局引入，全局覆盖config
              additionalData: `@import "bem.css/src/mixins.scss";@import 'xxx/variables.scss'`,
            },
          },
        ],
      },
    ],
  },
  // ...其他配置项
};
```

### 局部覆盖

```vue
<template>
  <div class="my-button"></div>
</template>

<script>
export default {
  name: "MyButton",
};
</script>

<style lang="scss" scoped>
// @import 'xxx/variables.scss';
// 或者直接定义变量覆盖
$namespace: "my";

@include b(button) {
  display: flex;
}
</style>
```

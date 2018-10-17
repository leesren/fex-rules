# Vue

## Vue 组件模板

```vue
<template>
    <!-- 模块类名 -->
    <div class="mod-xxx">your html content</div>
</template>
<script>
export default {
  components: {},
  mixins: [],
  data() {
    return {};
  },
  watch: {},
  created() {},
  mounted() {},
  methods: {},
  destroyed() {}
};
</script>

<style lang="scss" scoped>
@import "./index.scss";
</style>
```

#### `template`

必须加上模块的类名， 独立模块可以以 `mod-xx`, 开头；如果是页面 可以 `page-xx`

#### `JavaScript`

内容可多可少，但是 **必须** 按照这个顺序

#### `style` 

* 推荐写成单文件，不推荐 在 JS 中 `import 'xx.css'`
* 添加`scope` 作用域, 不加 `scope` 的用模块名包起来 
如：

```css
.mod-share{
    /** your css is here */
}
```
* 覆盖CSS用 `/deep/`
# Pagination
vue 分页组件，实现效果：首/尾页快捷跳转，自动省略号

默认使用了`Tailwind CSS`，可以自行更改样式，使用时候只需传入`current`（当前页）和`pageLen`（总页数）。

`pageAdd`,`pageDec`,`pageTo`可在父组件自行定义。

# 用例：
`parent.vue`
```js
<script setup>
import {ref} from 'vue'
const page = ref(1);
const pageLen = ref(10);

function pageAdd() {
  page.value += 1;
  if (page.value >= pageLen.value) {
    page.value = pageLen.value;
  }
}

function pageDec() {
  page.value -= 1;
  if (page.value <= 1) {
    page.value = 1;
  }
}

function pageTo(n) {
  page.value = n
}

</script>
<template>
<Pagination :current=page :pageLen=pageLen @pageAdd="pageAdd" @pageDec="pageDec" @pageTo="n=>{pageTo(n)}"></Pagination>
</template>

```

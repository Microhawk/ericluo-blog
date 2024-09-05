---
title: 将变量注入到CSS样式
date: 2022-01-11
description: 在VUE项目中将变量注入到CSS样式，通过变量来动态控制CSS样式
category: VUE
tags: [CSS]
---
## 1.完整示例：

1. 通过props接受来自父组件的数据
2. 新建计算属性`cssVars`
3. 在根`<div>`标签中注入`cssVars`。`:style="cssVars"`
4. 在`<style>`标签中进行引用`var(--progressWidth)`

```vue
<template>
  <div class="progress" :style="cssVars">
    <div class="rate" />
    <span>{{ content }}</span>
  </div>
</template>

<script>
export default {
  name: 'CustomProgress',
  props: {
    progressWidth: {
      type: String,
      default: '100%'
    },
    progressHeight: {
      type: String,
      default: '100%'
    },
    borderSize: {
      type: String,
      default: '1px'
    },
    borderColor: {
      type: String,
      default: '#304156'
    },
    borderRadius: {
      type: String,
      default: '5px'
    },
    rate: {
      type: Number,
      default: 0
    },
    rateColor: {
      type: String,
      default: '#ccc'
    },
    content: {
      type: String,
      default: ''
    },
    contentFontSize: {
      type: String,
      default: '15px'
    },
    contentFontColor: {
      type: String,
      default: '#304156'
    },
    contentTextAlign: {
      type: String,
      default: 'center'
    }
  },
  computed: {
    cssVars() {
      return {
        '--progressWidth': this.progressWidth,
        '--progressHeight': this.progressHeight,
        '--borderSize': this.borderSize,
        '--borderColor': this.borderColor,
        '--borderRadius': this.borderRadius,
        '--rate': this.rate + '%',
        '--rateColor': this.rateColor,
        '--contentFontSize': this.contentFontSize,
        '--contentFontColor': this.contentFontColor,
        '--contentTextAlign': this.contentTextAlign
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.progress{
  width: var(--progressWidth);
  height: var(--progressHeight);
  border: var(--borderSize);
  border-color: var(--borderColor);
  border-style: solid;
  border-radius: var(--borderRadius);
  overflow: hidden;
  position: relative;
  .rate {
    position: absolute;
    height: 100%;
    width: var(--rate);
    background-color: var(--rateColor);
  }
  span {
    position: absolute;
    height: 100%;
    width: 100%;
    line-height: 100%;
    font-size: var(--contentFontSize);
    color: var(--contentFontColor);
    text-align: var(--contentTextAlign);
  }
}
</style>
```




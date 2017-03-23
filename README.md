# ct-adc-ueditor

> 基于百度 Ueditor 的 VueJS 组件

## 快速开始

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

## 目录结构

核心代码目录如下:

```
ct-adc-ueditor/
...
└── src/
    └── component
        └── UEditor.vue
```

## 查看效果

View [demo](http://htmlpreview.github.io/?https://github.com/ct-adc/ct-adc-ueditor/blob/master/view/demo.html).

## API

- reset: Reset Ueditor, clear content.

- destroy: 销毁编辑器

- setContent: 设置内容(sContent | String)

- getContent: 获取富文本内容

> 当内容发生改变的时候，会 `emit` 一个 `change` 事件，并且会传递当前的内容最为参数


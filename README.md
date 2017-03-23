# ueditor

> A Vuejs Component based on UEditor

## Quick start

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

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## What's included

The core code directory:

```
ct-adc-ueditor/
...
└── src/
    └── component
        └── UEditor.vue
```

## View the end result

View [demo](./view/demo.html).

## Document

- reset()

Reset Ueditor, clear content.

- destroy()

Destroy Ueditor.

- setContent(sContent)

Set content.

- getContent()

Get current html content.

After content changed, it will emit a `change` event with current content.


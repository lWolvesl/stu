# ElementPlus

[回到主页](../../README.md)

## 安装

- npm
  - `npm install element-plus --save`

## 自动导入

#### 安装`unplugin-vue-components` 和 `unplugin-auto-import`这两款插件

`npm install -D unplugin-vue-components unplugin-auto-import`

### 完整导入

```ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```


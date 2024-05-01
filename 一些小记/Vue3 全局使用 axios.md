# 1. 全局引入

```js
import axios from './axios';

app.provide('$axios', axios);
```

# 2. 组件中使用

```js
import {inject} from 'vue'

const $axios = inject('$axios');
```



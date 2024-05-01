# 1. 安装

```shell
npm i qweather-icons
```

# 2. 引入

在 src/main.js 引入：

```js
import 'qweather-icons/font/qweather-icons.css'
```

# 3. 使用

```html
<i class="qi-301"></i>
```

# 4. 注意, 如果是从 api 获取的 icon 信息

```js
icon: 100 // 返回的是图片值

// 要在类名前加上 'qi-', 如: 'qi-100'
<i :class="`qi-${weatherData.icon}`"></i>
```



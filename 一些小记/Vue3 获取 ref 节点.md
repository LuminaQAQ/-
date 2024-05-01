# 1. html部分

```html
<section ref="firstScreen"></section>
```

# 2. js部分

```js
import { onMounted, ref } from 'vue'

export default {
  setup() {
    const firstScreen = ref(null);

    onMounted(() => {
      // 这里就能获取到了
      console.log(firstScreen.value);
    })

    return {
      // 注意要 return 出去
      firstScreen
    }
  }
}
```



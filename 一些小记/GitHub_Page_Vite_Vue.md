## 1. 安装gh-pages

```shell
npm install gh-pages --save-dev
```

## 2. 修改package.json

```shell
"scripts": {
        ...
     "deploy": "gh-pages -d dist"
} 
```

## 3. 修改 vite.config.js

```js
export default defineConfig({
  base: '/<repo name>/',
})
```

## 4. 打包

```shell
npm run build
```

## 5. 推送到github

```shell
npm run deploy
```



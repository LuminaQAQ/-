## 1. 安装gh-pages

```shell
npm install gh-pages --save-dev
```

## 2. 修改package.json

```shell
"private": true,
"homepage": "./",  

"scripts": {
        ...
     "deploy": "gh-pages -d build"
} 
```

## 3. 打包

```shell
npm run build
```

## 4. 推送到github

```shell
npm run deploy
```



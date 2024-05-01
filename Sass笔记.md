1. 变量定义
   
   ```sass
   // 1. 变量定义
   $color: #ccc
   
   // 2. 将局部变量设置为全局变量
   p {
       $abc: center !global;
   }
   
   div {
       text-align: $abc;
   }
   ```

2. 父选择器(**&**)
   
   ```sass
   // 1. 伪类样式
   div {
      &:hover {
          background-color: $color;
      }
   }
   
   // 2. 前面部分与后代样式同名
   .main-dialog {
     &-text {
         background-color: $color;
     }
   }
   ```

3. 属性嵌套
   
   ```sass
   div {
      font: {
          family: 'sans';
          size: 2rem;
          weight: bold;
      }
   }
   ```

4. 继承: **@extend**
   
   ```sass
   // 1.使用 .或# 本身的类, 会被编译在最终的css中
   @extend .class;
   @extend #id;
   
   // 2. 占位符: %; 定义的变量不会被编译至最终产物中, 只会是用时才会出现在子类
   @extend %class
   ```

5. 变量
   
   ```sass
   // 1. 数组
   $arr: 1px 3px 5px 10px;
   div {
      padding: $list;
    }
   
   // 2. 对象
   $obj: (align: center, color: red);
   div {
     color: map-get($obj, color);
   }
   
   // 3. 布尔
   $boolean: true;
   if $boolean {  }
   else { }
   
   // 4. 变量可使用变量
   $a: center;
   $b: $a;
   ```

6. 混入: **mixin**
   
   ```sass
   @mixin alert($color, $bgcolor) {
     color: $color;
     background-color: $bgcolor;
   }
   
   .alert-warning {
     @include alert(blue, yellow);
   }
   ```

7. 导入: **import(可用于脚手架导入函数所在文件)**
   
   ```sass
   // fileName: _file.scss, 导入可以忽略前面的_和扩展名
   import "file";
   
   file 内所有的样式直接被继承
   ```

8. 变量类型: **type-of()**
   
   ```sass
   // 1. 测试环境
   cmd -> sass -i
   
   // 1. 数字
   type-of(0)
   type-of(1px)
   
   // 2. 字符串
   type-of(Hello)   
   type-of("Hello")
   
   // 3. 数组
   type-of(1px solid #ccc)
   type-of(1px 2px)
   type-of(1px, 2px)    //  两个列表
   
   // 4. 颜色
   type-of(#ccc)
   type-of(red)
   
   ```

9. 数字运算注意
   
   ```sass
   // 1. 除法
   ( 8 / 2 ) // 正确
   8 / 2 // 表示的是其他意思
   
   // 2. 单位运算
   10px * 2px // res: 20 px*px (无效单位) 
   (10px / 2px) // res: 5 (单位消失) 
   
   // 3. 需要单位的运算
   10px * 2 // res: 20 px
   (10px / 2) // res: 5px
   
   ```

10. 数字函数
    
    ```sass
    // 1. abs()
    
    /* 2. 保留整数
     *    (1) round()
     *    (2) ceil()
     *    (3) floor()
     */
    
    // 3. percentage(): 百分比函数
    
    /* 4. 数字最大/小
     *    (1) min()
     *    (2) max()
     */
    
    ```

11. 字符串运算
    
    ```sass
    // 1. 减号
    a - b    // "a-b"
    
    //  2. 除号
    a / b    //  "a/b"
    
    // 3. 不能使用 字符串*字符串
    ```

12. 字符串函数
    
    ```sass
    // 1. 转大写
    to-upper-case()
    
    // 2. 转小写
    to-lower-case()
    
    // 3. 字符串长度
    str-length()
    
    // 4. 字符串出现位置(下标从1开始)
    str-index(var/str, keywords)
    
    // 5. 插入字符串
    str-insert(ori, insertStr, index)
    
    ```

13. 颜色函数
    
    ```sass
    // 1. 颜色变亮
    lighten(var/color, extent)
    
    // 2. 颜色变暗
    darkten(var/color, extent)
    
    // 3, 增加颜色纯度(饱和度)
    saturate(var/color, extent)
    
    // 4. 减少颜色纯度(饱和度)
    desaturate(var/color, extent)
    ```

14. 数组函数
    
    ```sass
    // 1. 数组长度
    length()
    
    // 2. 数组组合
    join(arr1, arr2)
    
    // 3. 查找所在值的索引
    index(arr, keyword)
    
    // 4. 查找所在索引的值
    nth(arr, index)
    
    // 5. 数组追加
    append(arr1, arr2[, sep])
    ```

15. 对象函数
    
    ```sass
    // 1. 获取指定键的值
    map-get(obj, key)
    
    // 2. 对象合并
    map-merge(obj1, obj2)
    
    // 3. 对象的指定键值移除
    map-remove(obj, key1[, key2])
    
    ```

16. 插值语法(模板字符串)
    
    ```sass
    $version: "0.0.1";
    $attr: border;
    
    // 1. 注释
    /* version: #{$version} */
    
    // 2. css 属性
    #{$attr}: 1px solid #ccc;
    ```

17. **@if**条件判断
    
    ```sass
    $theme: dark;
    
    @if $theme == dark {
        // ....
    } else if $theme == light {
        // ......
    } @else {
        // ......
    }
    ```

18. **@for**循环
    
    ```sass
    $ball-nums: 3;
    
    @for $i from 0 through $ball-nums {
      .ball#{$i} {
        $animation-time: 1s;
        $animation-delay: $animation-time / 4;
    
        left: $i * 2rem;
        animation: jump infinite 2s;
        animation-delay: $i * $animation-delay;
      }
    }
    
    @keyframes jump {
      50% {
        top: 0;
      }
    }
    ```

19. **@each**循环
    
    ```sass
    $alert-status: normal success warning;
    
    @each $key, $val in $alert-status {
      .alert-#{$k} {
        background-image: url(../images/#{$key}.png);
      }
    }
    ```

20. **@while**循环
    
    ```sass
    $end: 0;
    
    @while $end < 7 {
      .ball#{$end} {
        top: $end * 2;
      }
    
      $end: $end + 1;
    }
    ```

21. **function**自定义函数
    
    ```sass
    @function get-color($k) {
      @return map-get($colors, $k);
    }
    ```

22. 调试
    
    ```sass
    // 1. @debug 
    @debug "debug";
    
    // 2. @warn
    @warn "warn";
    
    // 3. @error
    @error "error";
    
    ```

# CSS 揭秘

## 编码技巧

### DRY (Don't Repeat Yourself)

尽量减少改动时需要编辑的地方

### 回退机制

为了确保网站不会在低版本浏览器中挂掉

- 利用样式的层叠机制实现回退

- 利用 `@sopports` 实现回退

### 避免不必要的媒体查询

- 使用百分比长度代替固定长度
- 使用 max-width 而不是 width
- 为替换元素（imag/video/iframe）设置 max-width
- background-size:cover
- 使用多列文本时指定 column-width 而不是 column-count，这样在小屏幕上就会自动显示为单列布局

## 实例

记一些比较基础的实例

### 半透明边框

```
border: 10px solid hsla(0,0%,100%,.5);
background: white;
background-clip: padding-box
```

### 多重边框

- box-shadow

- outline
    - 只能实现两层边框
    - 能够接受负值

### 灵活的背景定位

background-position 是以 padding box 为准的，可以通过将 background-origin 设置为 content box 来改变（其默认值为 padding box）

### 边框内圆角

box-shadow

### 条纹背景

水平条纹：

```
background: linear-gradient(#fb3 30%, #58a 30%);
background-size: 100% 30px;
```

垂直条纹：

```
background: linear-gradient(to right /* 或 90deg */, #fb3 30%, #58a 30%);
background-size: 100% 30px;
```

### 波点背景

```
background: #655;
background-image: radial-gradient(tan 30%, transparent 0),
                  radial-gradient(tan 30%, transparent 0);
background-size: 30px 30px;
background-position: 0 0, 15px 15px;
```

### 棋盘背景

用两个直角三角形拼成方块：

```
background: #eee;
background-image: linear-gradient(45deg, rgba(0,0,0,.25) 25%, transparent 0, transpanrent 75%, rgba(0,0,0,.25) d0),
background-image: linear-gradient(45deg, rgba(0,0,0,.25) 25%, transparent 0, transpanrent 75%, rgba(0,0,0,.25) d0),
background-size: 30px 30px;
background-position: 0 0, 15px 15px;
```

锥形渐变：

```
background: repeating-conic-gradient(#bbb 0, #bbb 25%, #eee 0, #eee 50%);
background-size: 30px 30px;
```

### 连续的图像边框

border-image 的工作原理是九宫格伸缩法，将图片切割成九块然后应用到元素边框响应的边和角

老式信封样式：

```
padding: 1em;
border: 1em solid transparent;
background: linear-gradient(white, white) padding-box, repeating-linear-gradient(-45deg, red 0, red 12.5%, transparent 0, transpanrent 25%, #58a 0, #58a 37.5%, transparent 0, transpanrent 50%) 0 / 5em 5em;

/* 也可以通过 border-image 实现*/
padding: 1em;
border: 1em solid transparent;
border-image: 16 repeating-linear-gradient(-45deg, red 0, red 1em, transparent 0, transpanrent 2em, #58a 0, #58a 3em, transparent 0, transpanrent 4em)
```

### 椭圆

border-radius 可以单独指定水平半径和垂直半径

`border-radius: 100px / 75px; /* 可以使用百分比使其能够自适应尺寸 */`

自适应的半椭圆：`border-radius: 50% / 100% 100% 0 0;`

### 平行四边形

`transform: skewX(-45deg);`

问题在于图形里面的文字也会跟着斜向拉伸

解决办法：

- 对内容再应用一次反向 skew() 变形

- 将样式应用到伪元素上，和内容分离

### 菱形

有两种方法：

- 用 rotate() 变形，scale() 放大

- `clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);`

### 切角效果

- linear-gradient

- SVG 与 border-image

- clip-path

### 梯形标签页

在三维中旋转一个矩形，从二维空间中观察到的就是一个梯形

`transform: perspective(.5em) rotateX(5deg);`

### 饼图

- 基于 transform：用一个元素作为容器，其他部分用伪元素、变形属性、CSS 渐变实现

- `background: conic-gradient(#655 sttr(data-value %), yellowgreen 0);`

### 投影

box-shadow: 水平偏移量 垂直偏移量 模糊距离 阴影尺寸 阴影颜色 insert/outsert

如果扩张半径的值正好等于模糊半径的负值，我们将看不到阴影

- 单侧投影：`box-shadow: 0 5px 4px -4px black;`
- 邻边投影：`box-shadow: 3px 3px 6px -3px black;` 水平和垂直偏移量需要大于或等于模糊半径的一半
- 双侧投影：`box-shadow: 5px 0 5px -5px black, -5px 0 5px -5px black;` 运用两次单侧投影

### 不规则投影

使用滤镜中的 drop-shadow

滤镜：`filter: blur() grayscale() drop-shadow();`

使用 drop-shadow 时任何非透明部分都会被打上阴影，包括文字，而且也不能使用 text-shadow 取消

### 垂直居中

- 绝对定位：
    ```
    main {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    ```

- 相对于视口:
    ```
    main {
        width: 18em;
        padding: 1em 1.5em;
        margin: 50vh auto 0;
        transform: translateY(-50%);
    }
    ```

- Flexbox
    ```
    body {
        display: flex;
        min-height: 100vh;
        margin: 0;
    }

    main {
        margin: auto;
    }
    ```

- `align-self: center;`

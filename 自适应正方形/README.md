### 记 CSS 实现正方行自适应

平时我们会遇到一种情况，就是我们需要一个正方形，可以很简单

```HTML

  <style>
    .square {
      width: 100px;
      height: 100px;
    }
  </style>

  <body>
    <div class="square"></div>
  </body>

```

但我想在移动端使用，并且需要正方形随着屏幕宽度变化，我把代码改成这样

方法一： 利用 `padding` 来实现

```HTML

  <style>
    .square {
      padding-bottom: 100%;
      width: 100%;
      height: 0;
    }
  </style>

  <body>
    <div class="square"></div>
  </body>

```
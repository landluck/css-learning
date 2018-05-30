## 双飞翼布局 

双飞翼布局，是一种页面布局的形象的表达。具体表现形式为 两边顶宽，中间自适应宽度的三栏布局，中间栏要放在HTML文档流的最前，优先渲染

#### 实现方式

  `float` 浮动流

    页面基本布局很简单，总共有三栏，中间栏位于文档流的最前面，优先渲染

    ```
      <body>
        <section class="container">
          <div class="main"> // 中间栏，位于文档流的最前面
            <div class="main-inner"> // 中间栏内容
            </div>
          </div>
          <div class="sub"></div> // sub 左边栏
          <div class="extra"></div> // extra 右边栏
        </section>
      </body>

    ```

  1. 为方便显示，清除`margin`和`padding`, 指定高度给`container`,指定背景颜色给三栏

    ```
      /* 清除margin和padding */

      * { 
        margin: 0;
        padding: 0;
      }
      /* 方便显示效果，指定高度，这里应该内容自动撑高 */

      .container {
        height: 600px;
      }
      .main {
        background: red;
      }
      .sub {
        background: blue;
      }
      .extra {
        background: yellow;
      }
    ```

  2. 三栏全部左浮动，指定高度

    ```
      /* 三栏都左浮动，指定高度 */
      .main, .sub, .extra {
        float: left;
        height: 100%;
      }
    ```

  3. 中间栏宽度`100%`，占满所有宽度， 排在第一行 

    ```
      .main {
        width: 100%;
      }
    ```

  4. `sub` 左边栏 指定宽度 `150px`, 这个时候，因为 `main` 中间栏已经占满所有宽度，所以 `sub` 左边栏被挤出屏幕， 又因为之前统一左浮动，所以，
    所以 `sub` 从 `main` 下面，第二行排列， 为了 `sub` 左边栏能够排到第一行的最左边， `sub` 左边栏 指定 `margin-left: -100%;` `-100%` 即为整个屏幕的宽度

    ```
    .sub {
        width: 150px;
        margin-left: -100%;
      }
    ```

  5.  同理， `extra` 右边栏 指定宽度 `200px`,  `main` 中间栏还是已经占满所有宽度，所以 `extra` 右边栏被挤出屏幕， 又因为之前统一左浮动，所以，
    所以 `extra` 从 `main` 下面，第二行排列， 为了 ``extra` 右边栏能够排到第一行的最右边， `extra` 右边栏 指定 `margin-left: -200px`; 

    ```
      .extra {
        width: 200px;
        margin-left: -200px;
      }
    ```

  6. 为了中间 `main` 内容不被遮挡，直接在中间 `main` 内部创建子 `main-inner` 用于放置内容，在该子 `main-inner` 里用 `margin-left` 和 `margin-right` 为左右两栏留出位置。

    ```
      .main-inner {
        margin-left:150px;
        margin-right: 200px;
        height: 100%;
      }
    ```

  `flex` 流

    页面布局基本不变

    ```
      <body>
        <section class="container">
          <div class="main"> // 中间栏，位于文档流的最前面
          </div>
          <div class="sub"></div> // sub 左边栏
          <div class="extra"></div> // extra 右边栏
        </section>
      </body>
    ```

  1. 为方便显示，清除`margin`和`padding`, 指定高度给`container`,指定背景颜色给三栏

    ```
      /* 清除margin和padding */

      * { 
        margin: 0;
        padding: 0;
      }
      /* 方便显示效果，指定高度，这里应该内容自动撑高 */

      .container {
        height: 600px;
      }
      .main {
        background: red;
      }
      .sub {
        background: blue;
      }
      .extra {
        background: yellow;
      }
    ```

  2. 父元素 `display: flex;` 让子元素排在一行

    ```
      .container {
        display: flex;
      }
    ```

  3. 中间栏 `flex-grow: 1;` 自动占满剩余所有宽度

    ```
      .main {
        flex-grow: 1;
      }
    ```

  4. `sub` 左边栏 指定 `flex-grow: 0;` 不占满剩余宽度, `flex-shrink: 0;` 如果空间不足，该栏不缩小, `flex-basis: 150px;` 指定固定宽度为`150px;`  
      `order: -1,` `flex` 布局中，`order` 属性定义项目的排列顺序。数值越小，排列越靠前，默认为`0`。此处为 `-1` ， 排在最左边

    ```
      .sub {
        flex: 0 0 150px;
        order: -1;
      }
    ```

  5. `extra` 右边栏 `order` 为 `1` ， 排在最右边

    ```
      .extra {
        flex: 0 0 200px;
        order: 1;
      }
    ```

## 圣杯布局
  圣杯布局和双飞翼布局解决的问题是一样的，就是两边顶宽，中间自适应的三栏布局，中间栏要在放在文档流前面以优先渲染。圣杯布局和双飞翼布局解决问题的方案在前一半是相同的，也就是三栏全部`float`浮动，但左右两栏加上负margin让其跟中间栏`div`并排，以形成三栏布局。不同在于解决 "中间栏`div`内容不被遮挡" 问题的思路不一样：

  >圣杯布局，为了中间div内容不被遮挡，将中间div设置了左右padding-left和padding-right后，将左右两个div用相对布局position: relative并分别配合right和left属性，以便左右两栏div移动后不遮挡中间div


    ```
        <body>
          <section class="container">
            <div class="main"> // 中间栏，位于文档流的最前面
              </div>
            </div>
            <div class="sub"></div> // sub 左边栏
            <div class="extra"></div> // extra 右边栏
          </section>
        </body>
    ```

  1. 为方便显示，清除`margin`和`padding`, 指定高度给`container`,指定背景颜色给三栏

  ```
    /* 清除margin和padding */

    * { 
      margin: 0;
      padding: 0;
    }
    /* 方便显示效果，指定高度，这里应该内容自动撑高 */

    .container {
      height: 600px;
    }
    .main {
      background: red;
    }
    .sub {
      background: blue;
    }
    .extra {
      background: yellow;
    }
  ```
  2. 三栏全部左浮动，指定高度

  ```
    /* 三栏都左浮动，指定高度 */
    .main, .sub, .extra {
      float: left;
      height: 100%;
    }
  ```
  3. 中间栏宽度`100%`，占满所有宽度， 排在第一行 

  ```
      .main {
      width: 100%;
    }
  ```
  4. `sub` 左边栏 指定宽度 `150px`, 这个时候，因为 `main` 中间栏已经占满所有宽度，所以 `sub` 左边栏被挤出屏幕， 又因为之前统一左浮动，所以，
      所以 `sub` 从 `main` 下面，第二行排列， 为了 `sub` 左边栏能够排到第一行的最左边， `sub` 左边栏 指定 `margin-left: -100%;` `-100%` 即为整个屏幕的宽度
  ```
  .sub {
      width: 150px;
      margin-left: -100%;
    }
  ```

  5.  同理， `extra` 右边栏 指定宽度 `200px`,  `main` 中间栏还是已经占满所有宽度，所以 `extra` 右边栏被挤出屏幕， 又因为之前统一左浮动，所以，
      所以 `extra` 从 `main` 下面，第二行排列， 为了 ``extra` 右边栏能够排到第一行的最右边， `extra` 右边栏 指定 `margin-left: -200px`; 
  ```
    .extra {
      width: 200px;
      margin-left: -200px;
    }
  ```
  6. 为了中间 `main` 内容不被遮挡，需要给父容器`container` 添加 `padding`。

  ```
    .container{
      padding: 0 200px 0 150px; 
      height: 100%;
    }
  ```
  7. 这个时候因为两边`padding`的原因，`sub` 栏 和 `extra` 栏的位置已经被挤到中间，为了回到预定的位置，使用 `position` 定位

  ```
    .sub{
      position: relative;
      left: -150px;
    }

    .extra {
      position: relative;
      right: 200px;
    }
  ```

## 总结
  总得来说，这三种方法，都是为了实现同一种布局，除了`flex`布局，其余的方式，所有的浏览器都支持，不会有兼容问题

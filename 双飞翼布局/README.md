## 双飞翼布局 

双飞翼出自淘宝UED，应该是一种页面布局的形象的表达。具体表现形式为 两边顶宽，中间自适应宽度的三栏布局，中间栏要放在HTML文档流的最前，优先渲染

#### 实现方式

    - float 浮动流

      具体实现思路

      ```
        <style>
          /* 清除margin和padding */
          * { 
            margin: 0;
            padding: 0;
          }
          /* 方便显示效果，指定高度，这里应该内容自动撑高 */
          .container {
            height: 600px;
          }
          /* 三栏都左浮动，指定高度 */
          .main, .sub, .extra {
            float: left;
            height: 100%;
          }
          /* 中间栏宽度100%，占满所有宽度， 排在第一行 */ 
          .main {
            width: 100%;
            background: green;
          }
          /* sub 左边栏 指定宽度 150px, 这个时候，因为 main 中间栏已经占满所有宽度，所以 sub 左边栏被挤出屏幕， 又因为之前统一左浮动，所以，
             所以 sub 从 main 下面，第二行排列， 为了 sub 左边栏能够排到第一行的最左边， sub 左边栏 指定 margin-left: -100%; 
             -100% 即为整个屏幕的宽度*/
          .sub {
            width: 150px;
            margin-left: -100%;
            background: gray
          }
           /* 同理， extra 右边栏 指定宽度 200px,  main 中间栏还是已经占满所有宽度，所以 extra 右边栏被挤出屏幕， 又因为之前统一左浮动，所以，
             所以 extra 从 main 下面，第二行排列， 为了 extra 右边栏能够排到第一行的最右边， extra 右边栏 指定 margin-left: -200px; 
             -100% 即为整个屏幕的宽度*/
          .extra {
            width: 200px;
            margin-left: -200px;
            background: blue;
          }
          /* 为了中间 main 内容不被遮挡，直接在中间div内部创建子div用于放置内容，在该子div里用margin-left和margin-right为左右两栏div留出位置。 */
          .main-inner {
            margin-left:150px;
            margin-right: 200px;
            height: 100%;
            word-break: break-all;
          }
        </style>
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
    - flex 弹性布局流
      
        具体思路

          ```
            <style>
              /* 清除margin和padding */
              * {
                padding: 0;
                margin: 0;
              }
              /* 父元素 display: flex; 让子元素排在一行 */
              .container {
                display: flex;
                height: 600px;
              }
              /* 中间栏 flex-grow: 1; 自动占满剩余所有宽度 */
              .main {
                flex-grow: 1;
                background: blue;
                height: 600px;
              }
              /* sub 左边栏 指定 flex-grow: 0; 不占满剩余宽度, flex-shrink: 0; 
                 如果空间不足，该栏不缩小, flex-basis: 150px; 指定固定宽度为150px;  
                 order: -1, flex 布局中，order 属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。此处为 - 1 ， 排在最左边
                 */
              .sub {
                flex: 0 0 150px;
                background: red;
                order: -1;
              }
              /*
                order 为 1 ， 排在最右边
              */
              .extra {
                flex: 0 0 200px;
                background: yellow;
                order: 1;
              }
            
            </style>
            <section class="container">
              <div class="main">
              </div>
              <div class="sub"></div>
              <div class="extra"></div>
            </section>
          ```
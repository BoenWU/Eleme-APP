## vue2 +vue-router2 + es6 +webpack 高仿饿了么app
喜欢的请点心，关注，star ,fork,谢谢大家！

### 项目技术架构

* vue-cli
* vue
* vue-resource
* vue-router
* betterScroll
* less
* webpack

### 项目效果预览

* 加入购物车

    ![shopcart](https://camo.githubusercontent.com/a2945c488a774a2a329a97d8334413a3154ee9be/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f343234393232332d386434363262393331353064623234652e6769663f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970)

* 分页效果

    ![app](https://camo.githubusercontent.com/252801c20f20a347263f26a3bc64accf3c38926e/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f343234393232332d663039323162343437343066366434662e6769663f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970)
    
    
### 安装

- 项目地址：（git clone）
- 通过npm安装本地服务第三方依赖模块(需要已安装Node.js)

  ```
  npm install
  ```
  
- 启动服务(http://localhost:8080)

  ```
  npm run dev
  ```
  
-  发布代码

  ```
  npm run build
  ```
  
### 目录结构
***
<pre>
├── build              // 构建服务和webpack配置
├── config             // 项目不同环境的配置
├── rescource          // 项目资源
├── index.html         // 项目入口文件
├── package.json       // 项目配置文件
├── src                // 生产目录
│  ├── assets          // 图片资源
│  ├── common          // 公共的css js 资源
│  ├── components      // 各种组件
│  ├── App.vue         // 主页面
│  └── main.js         // Webpack 预编译入口
</pre>

### 实现的功能
***
* 商品滚动 ，商品滚轮滚动
* 商品联动
* 加入购物车，移除购物车
* 显示评论 评论筛选
* 图片左右滑动
* 商品详情  父子组件的通信
* 等等

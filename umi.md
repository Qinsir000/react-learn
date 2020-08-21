# umi

## 约定式路由

###   路由匹配

-  工程中的 pages 文件夹中存放的是页面。如果工程包含 src 目录，则 src/pages 是页面文件夹。
-  页面的文件名，以及页面的文件路径，是该页面匹配的路由
-  如果页面的文件名是 index，则可以省略文件名（首页）(注意避免文件名和当前目录中的文件夹名称相同)
-  如果 src/layout 目录存在，则该目录中的 index.js 表示的是全局的通用布局，布局中的 children 则会添加具体的页面。
-  如果 pages 文件夹中包含\_layout.js，则 layout.js 所在的目录以及其所有的子目录中的页面，共用该布局。
- 404 约定，umi 约定，pages/404.js，表示 404 页面，如果路由无匹配，则会渲染该页面。该约定在开发模式中无效，只有部署后生效。
- ![a3d369b1142c9a1d91531565b286d8af.png](en-resource://database/495:1)

-  使用\$名称，会产生动态路由
- ![23270ede8ba7c85c27d1cf62b011ce3b.png](en-resource://database/497:1)

###   路由跳转

-  跳转链接：  导入`umi/link`，`umi/navlink`
-  代码跳转：  导入`umi/router`

> 导入模块时，@表示 src 目录
>  ![12247b60acafc8709bd5cad65c84accf.png](en-resource://database/496:1)

###   路由信息的获取

所有的页面、布局组件，都会通过属性，收到下面的属性
- match：等同于 react-router 的 match
- history：等同于 react-router 的 history（history.location.query 被封装成了一个对象，使用的是 query-string 库进行的封装）
- location：等同于 react-router 的 location（location.query 被封装成了一个对象，使用的是 query-string 库进行的封装）
- route：对应的是路由配置
如果需要在普通组件中获取路由信息，则需要使用 withRouter 封装，可以通过`umi/withRouter`导入

## 配置式路由

当使用了路由配置后，约定式路由全部失效。
两种方式书写 umi 配置：
1.  使用根目录下的文件`.umirc.js`
2.  使用根目录下的文件`config/config.js`
进行路由配置时，每个配置就是一个匹配规则，并且，每个配置是一个对象，对象中的某些属性，会直接形成 Route 组件的属性
![ea078e0dc7200c27637deac604ddf6bc.png](en-resource://database/510:1)
注意：
- component 配置项，需要填写页面组件的路径，路径相对于 pages 文件夹
-  如果配置项没有 exact，则会自动添加 exact 为 true
-  每一个路由配置，可以添加任何属性
- Routes 属性是一个数组，数组的每一项是一个组件路径，路径相对于项目根目录，当匹配到路由后，会转而渲染该属性指定的组件，并会将 component 组件作为 children 放到匹配的组件中
路由配置中的信息，同样可以放到约定式路由中，方式是，为约定式路由添加第一个文档注释（注释的格式的 YAML），需要将注释放到最开始的位置

### YAML 格式

-  键值对，冒号后需要加上空格
-  如果某个属性有多个键或多个值，需要进行缩进（空格）

- YAML
  ![95aefef089b5c78f089842af1f3dc10c.png](en-resource://database/512:1)

## 使用 dva

文档： https://umijs.org/zh/plugin/umi-plugin-react.html

### dva 在 umi 中将模型分为两种：

1. **全局模型**：所有页面通用，工程一开始启动后，模型就会挂载到仓库
2. **局部模型**：只能被某些页面使用，访问具体的页面时才会挂载到仓库

**umi 里的 dva 里的写法和 dva 里的相同，具体可查看官网文档**

### 开启插件

依然是在.umirc.js 里操作

![44eb641e0f7f8d8a2e177adfc0256ff3.png](en-resource://database/514:1)

### 全局模型

在`src/models`目录下定义的 js 文件都会被看作是全局模型，默认情况下，模型的命名空间和文件名一致。

![e68991023ff3fd5d2c2fe358d2e8e8b2.png](en-resource://database/516:1)

### 局部模型

**特点**局部模型定义在 pages 文件夹或其子文件夹中，在哪个文件夹定义的模型，会被该文件夹中的所有页面以及子页面、以及该文件夹的祖先文件夹中的页面所共享。也就是说，凡是这个页面的直链（父子爷孙）都可共享。

局部模型的定义和全局模型的约定类似，需要创建一个 models 文件夹

![f5fd5dd251f2851c1576590d0a9ee7a2.png](en-resource://database/518:1)

## 代理（proxy）

代理用于解决跨域问题

配置`.umirc.js`中的 proxy

![7745d2ac92224346988edbb753fd4675.png](en-resource://database/522:1)

## 数据模拟（mock）

用于解决前后端协同开发的问题
数据模拟可以让前端开发者在开发时，无视后端接口是否真正完成，因为使用的是模拟的数据

umijs 约定：
1. mock 文件夹中的文件
2. src/pages 文件夹中的\_mock.js 文件
以上两种 JS 文件，均会被 umijs 读取，并作为数据模拟的配置
可以自行发挥，添加模拟数据，通常，我们会和 mockjs 配合。

![36aa8dcc73b02d7bea29071924647c86.png](en-resource://database/524:1)

具体 mock 语法详见文档（**https://github.com/nuysoft/Mock/wiki/Syntax-Specification**）

##   额外的约定文件

- src/pages/document.ejs:  页面模板文件
- src/global.js：在 umi 最开始启动时运行的 js 文件
- src/app.js：作运行时配置的代码
  - patchRoutes:  函数，该函数会在 umi 读取完所有静态路由配置后执行
  - dva
    - config：  相当于 new dva(配置)
    - plugins：  相当于 dva.use(插件)
- .env:  配置环境变量，这些变量会在 umi 编译期间发挥作用
  - UMI_ENV：umi 的环境变量值，可以是任意值，该值会影响到.umirc.js
  - PORT
  - MOCK

## umirc 配置

书写在.umirc.js 文件中的配置
- plugins：配置 umijs 的插件
- routes：配置路由（会导致约定式路由失效）
- history：history 对象模式（默认是 browser）
- outputPath：使用 umi build 后，打包的目录名称，默认./dist
- base:  相当于之前 BrowserRouter 中的 basename
- publicPath:  指定静态资源所在的目录
- exportStatic:  开启该配置后，会打包成多个静态页面，每个页面对应一个路由，开启多静态页面应用的前提条件是：没有动态路由

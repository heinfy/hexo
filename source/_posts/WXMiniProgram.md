---
title: WXMiniProgram
categories:
  - 微信
tags:
  - 微信小程序
abbrlink: 6316
date: 2020-06-22 18:00:46
---

<!-- more -->

## 1.开发准备

- [账号申请](https://mp.weixin.qq.com/cgi-bin/registermidpage?action=index&lang=zh_CN)

  - 选择注册账号类型 —— 小程序
  - 账号信息
  - 邮箱激活
  - 信息登记

- 绑定开发者

  - [登录小程序](https://mp.weixin.qq.com/)
  - 管理员授权
  - 添加成员

- 获取 AppID

  - 设置——开发设置

- 开发工具

  - 下载—扫码登录—选择小程序—创建小程序—预览模板界面

- [官方文档](https://developers.weixin.qq.com/miniprogram/dev/index.html?t=2018413)

## 2.结构分析

以下是小程序的基本目录结构，也可以根据需要任意添加其它目录，如资源目录 assets、第三方库 vendors、扩展目录 extends 等。

```bash
├── app.js ...................小程序入口文件
├── app.json .................小程序全局配置
├── app.wxss .................小程序全局样式
├── pages ....................所有页面目录
│   ├── index ................index页面目录
│   │   ├── index.js .........index页面业务逻辑
│   │   ├── index.wxml .......index页面布局结构
│   │   └── index.wxss .......index页面布局样式
│   └── logs .................logs页目录
│       ├── logs.js ..........logs页面业务逻辑
│       ├── logs.json ........logs页面配置文件
│       ├── logs.wxml ........logs页面布局结构
│       └── logs.wxss ........logs页面布局样式
├── project.config.json ......开发工具配置文件
└── utils ....................公共逻辑
    └── util.js ..............实用工具
```

==主体 app：一个小程序主体部分由三个文件组成，放在项目的根目录（如果项目使用 gulp 或者 webpack 打包，则需要在`project.config.js` 指定 `"miniprogramRoot": "路径“` 属性）==

### 2.1 全局配置

通过 app.json 文件对小程序进行全局配置，如页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等。

**app.json 配置清单**

| 属性           | 类型         | 必填 | 描述                 |
| -------------- | ------------ | ---- | -------------------- |
| pages          | String Array | 是   | 设置页面路径         |
| window         | Object       | 否   | 设置默认窗口表现     |
| tabBar         | Object       | 否   | 设置底部 tab 表现    |
| networkTimeout | Object       | 否   | 设置网络超时时间     |
| debug          | Boolean      | 否   | 设置是否开启调试模式 |

app.json 示例

```json
{
  "pages": ["pages/index/index", "pages/detail/index"]
}
```

2. **window**

对象类型，用于设置小程序的状态栏、导航条、标题、窗口背景色。

| 属性                         | 类型     | 默认值  | 描述                               | 兼容 |
| ---------------------------- | -------- | ------- | ---------------------------------- | ---- |
| navigationBarBackgroundColor | HexColor | #000000 | 导航栏背景颜色，如"#000000"        | -    |
| navigationBarTextStyle       | String   | white   | 导航栏标题颜色，仅支持 black/white |      |
| navigationBarTitleText       | String   |         | 导航栏标题文字内容                 |      |
| backgroundColor              | HexColor | #ffffff | 窗口的背景色                       |      |

查看 window[更多配置](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#window)属性

编辑 app.json，添加字段 windows

```json
{
  "window": {
    "navigationBarBackgroundColor": "#262626",
    "navigationBarTitleText": "XXX",
    "navigationBarTextStyle": "white",
    "backgroundColor": "#F0F0F0"
  }
}
```

3. **tabBar**

对象类型，配置项指定 tab 栏的表现，以及 tab 切换时显示的对应页面。

| 属性            | 类型     | 必填 | 默认值 | 描述                                     |
| --------------- | -------- | ---- | ------ | ---------------------------------------- |
| color           | HexColor | 是   |        | tab 上的文字默认颜色                     |
| selectedColor   | HexColor | 是   |        | tab 上的文字选中时的颜色                 |
| backgroundColor | HexColor | 是   |        | tab 的背景色                             |
| borderStyle     | String   | 否   | black  | tabbar 上边框的颜色， 仅支持 black/white |
| list            | Array    | 是   |        | tab 的列表，最少 2 个、最多 5 个         |
| position        | String   | 否   | bottom | 可选值 bottom、top                       |

1. 当设置 position 为 top 时，将不会显示 icon
2. tabBar 中的 list 是一个数组，**只能配置最少 2 个、最多 5 个 tab**，tab 按数组的顺序排序。

其中 list 接受一个数组，数组中的每个项都是一个对象，其属性值如下：

| 属性             | 类型   | 必填 | 描述                                                                                                      |
| ---------------- | ------ | ---- | --------------------------------------------------------------------------------------------------------- |
| pagePath         | String | 是   | 页面路径，必须在 pages 中先定义                                                                           |
| text             | String | 是   | tab 上按钮文字                                                                                            |
| iconPath         | String | 否   | 图片路径，icon 大小限制为 40kb，建议尺寸为 81px \* 81px，当 postion 为 top 时，此参数无效，不支持网络图片 |
| selectedIconPath | String | 否   | 选中时的图片路径，icon 大小限制为 40kb，建议尺寸为 81px \* 81px ，当 postion 为 top 时，此参数无效        |

### 2.2 页面配置

> 每个页面可以有不同的表现，通过 pages 目录下的 .json 文件，如 logs.json ，实现页面的局部配置。
>
> 但是只能设置 app.json 中的 window 配置项的内容，页面中配置项会覆盖 app.json 的 window 中相同的配置项。

| 属性                         | 类型     | 默认值 | 描述                               |
| ---------------------------- | -------- | ------ | ---------------------------------- |
| navigationBarBackgroundColor | HexColor | #000   | 导航栏背景颜色                     |
| navigationBarTextStyle       | String   | white  | 导航栏标题颜色，仅支持 black/white |
| navigationBarTitleText       | String   |        | 导航栏标题文字内容                 |
| backgroundColor              | HexColor | #fff   | 窗口的背景色                       |

```
// 在 pages/center 目录下创建 index.json
FC/pages/center
├── index.js
├── index.json
└── index.wxml
```

如下所示，编辑 index.json

```json
{
  "navigationBarBackgroundColor": "#ffffff",
  "navigationBarTextStyle": "white",
  "navigationBarTitleText": "我的"
}
```

### 2.3 适配、样式和组件

**一句话：开发小程序时所有屏幕宽度都是 750rpx。**

**rpx（responsive pixel）可以根据屏幕宽度进行自适应。规定所有屏幕宽为 750rpx。**

rpx 与 px 的换算关系：

| 设备           | 屏幕尺寸 | rpx 换算 px (屏幕宽度/750) | px 换算 rpx (750/屏幕宽度) |
| -------------- | -------- | -------------------------- | -------------------------- |
| iPhone5        | 320px    | 1rpx = 0.42px              | 1px = 2.34rpx              |
| 小米 MIX 2S    | 360px    | 1rpx = 0.48px              | 1px = 2.083rpx             |
| iPhone6        | 375px    | 1rpx = 0.5px               | 1px = 2rpx                 |
| iPhone6 Plus   | 414px    | 1rpx = 0.552px             | 1px = 1.81rpx              |
| HUAWEI Mate 10 | 480px    | 1rpx = 0.64px              | 1px = 1.562rpx             |

- 样式导入

  使用 @import 语句可以导入外联样式表， @import 后跟需要导入的外联样式表的相对路径，用 ; 表示语句结束。

  ```css
  /** common.wxss **/
  page {
    background: #f0f0f0;
  }
  ```

  ```css
  /** index.wxss **/
  @import 'common.wxss';
  page {
    background: #f2f2f2;
  }
  ```

- 内联样式

- 外部样式

**image 相当于 html 中的 img 标签，用来加载图片。**

```html
<!-- 通过 src 属性加载图片 -->
<!-- 通过 mode 属性调整 图片的的显示方式 (裁切/缩放) -->
<!-- image 组件默认宽度为300px，默认高度为225px -->
<image src="../../static/uploads/item_1.png" mode="aspectFit"></image>
```

关于 [mode 有效值](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)

**text 相当于 html 中的 span，用来定义文本。**

**view 相当于 html 中的 div，一般做为容器出现。**

**swiper 滑块组件，可以用来实现类似轮播图布局效果。**

## 3.基本语法

#### 数据绑定

类似`vue`

```js
// pages/demo/data.js
Page({
    // 通过 data 属性，初始化页面中用到的数据
    data: {
        message: 'hello world!'
        user: {
            name: '小明',
            age: 16
        },
        courses: ['wxml', 'wxss', 'javascript']
    }
});
```

```html
<!-- pages/demo/data.wxml -->
<text class="msg">{{message}}</text>
<text>我叫{{user.name}}， 我今年{{user.age}}岁了， 我在学习{{courses[0]}}课程。</text>
```

#### 列表数据

```js
// pages/demo/data.js
Page({
  // 通过 data 属性，初始化页面中用到的数据
  data: {
    brands: [
      {
        name: '耐克',
        origin: '美国',
        category: ['男装', '女装', '鞋', '体育用品']
      },
      {
        name: 'SK-II',
        origin: '韩国',
        category: ['防晒霜', '面膜', '洗护']
      }
    ]
  }
});
```

```html
<!-- pages/demo/data.wxml -->
<view wx:for="{{brands}}" wx:for-index="k" wx:for-item="v">
  <view>
    <text>{{k+1}}</text>
    <text>{{v.name}}</text>
    <text>{{v.origin}}</text>
  </view>
  <view>
    <text wx:for="{{v.category}}">{{item}}</text>
  </view>
</view>
```

wx:for 属性将当前组件按着数组的长度动态创建，并且通过 index 变量可以访问到数组的索引值，通过 item 变量可以访问到单元值。

#### block

```html
<!-- pages/demo/data.wxml -->
<block wx:for="{{users}}">
  <text>{{item.name}}</text>
  <text>{{item.age}}</text>
</block>
```

#### 条件数据

```html
<view wx:for="{{users}}">
  <text>{{index+1}}</text>
  <text>{{item.name}}</text>
  <text>{{item.age}}</text>
  <text wx:if="{{item.age <= 14}}">儿童</text>
  <text wx:elif="{{item.age <= 18}}">未成年</text>
  <text wx:else>成年人</text>
</view>
```

```js
Page({
  data: {
    users: [
      { name: '小明', age: 18 },
      { name: '小红', age: 13 },
      { name: '小丽', age: 19 }
    ]
  }
});
```

#### 事件触发

**语法格式： bind:事件名称="回调函数"**

```html
<button type="primary" bind:tap="sayHi">点我试试</button>
<input type="text" bind:focus="sayHi" bind:blur="sayBye" />
```

```js
// pages/demo/data.js
Page({
  sayHi: function () {
    console.log('Hi~');
  },
  sayHi: function () {
    console.log('Hi~');
  },
  sayBye: function (ev) {
    // ev  时间对象
    console.log(ev);
    console.log('Bye~');
  }
});
```

#### 事件冒泡

- 子元素用 bindtap 绑定事件后，执行的时候，会冒泡到父元素（触发父亲元素上绑定的 bindtap 事件）

- 如果不想冒泡到父元素，可以用 catchtap 代替

```html
<botton catchtap="goDetail">catchtap</botton>
<botton bindtap="goDetail">bindtap</botton>
```

## 4.生命周期

<img src="WXMiniProgram/%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png" alt="微信小程序的生命周期" style="zoom:67%;" />

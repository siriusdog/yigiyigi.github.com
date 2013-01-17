---
layout: default
title:  远离尘世的小木屋
---

ios上有添加到主屏的功能。从这个主屏按钮启动的webapp就像native app一样会有一个启动画面，这是ios提供的系统机制，在这个画面出现的时候页面没有加载完也没有渲染。添加到主屏之后，从主屏的shortcut icon启动webapp时将会没有顶部地址栏和底部的导航控制栏。这里会详细描述添加到主屏的一系列设置。


顶部地址栏

底部导航栏

   第一步：让webapp支持从主屏以app形式打开。
支持版本：iPhone OS 2.1
    方法：<meta name="apple-mobile-web-app-capable" content="yes" />
否则即使用户点击了添加到主屏，再次点击主屏上的这个webapp的icon，仍然会从safari里打开。

   第二步：设置添加到桌面的图标
如果不设置将会默认将屏幕的截图作为icon。
1、设置图片方法：<link rel="apple-touch-icon" href="icon.png" />  按照此方法设置icon，ios将会给图片加上默认高光

带高光效果的icon
2、如果不想加高光，那么不带高光的设置方法<link rel="apple-touch-icon-precomposed" href="icon.png" />
Ps：Ios2.0之后支持<link rel="apple-touch-icon" href=" apple-touch-icon-precomposed.png " /> ，直接将图片命名为apple-touch-icon-precomposed.png不用改变rel就可以强制不加高光效果。


不带高光效果的icon
3、如果你的代码需要同时适配不同设备，比如ipad、ipod、iphone，ios4.2之后提供了支持多设备的多icon方案。
只需在原有的meta标签上添加一个size属性
<link rel="apple-touch-icon" href="touch-icon-iphone.png" />
<link rel="apple-touch-icon" sizes="72x72" href="touch-icon-ipad.png" />
<link rel="apple-touch-icon" sizes="114x114" href="touch-icon-iphone-retina.png" />
<link rel="apple-touch-icon" sizes="144x144" href="touch-icon-ipad-retina.png" />
规则：
1、  设置之后设备将自动匹配最合适尺寸的图片，将其作为桌面图标。如果没有设置size属性，默认的size将会是57*57
2、  如果没有完全匹配尺寸的icon，系统会先选择最小的大于推荐尺寸的那张图片，如果没有大于推荐尺寸的图片系统会选择最大的图片。
3、  如果没有通过link标签显示的设置icon，系统还会去网站的root目录找名为apple-touch-icon...或 apple-touch-icon-precomposed…为前缀的图片。比如一个设备非retina的iphone，其推荐尺寸为57*57，系统查找图片的顺序将会是：(优先级逐渐下降)
   apple-touch-icon-57x57-precomposed.png
   apple-touch-icon-57x57.png
   apple-touch-icon-precomposed.png
   apple-touch-icon.png

   第三步：设置主屏启动的状态栏
设置从主屏启动的webapp的顶部状态栏可以让webapp做到真正全屏。

顶部状态栏
设置方法：
1、<meta name="apple-mobile-web-app-status-bar-style" content="black" />
        不设置此meta效果和设置为black一样。

默认的black样式
2、<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
设置为black-translucent效果是实现webapp真正全屏、信息栏半透明。

black-translucent

   第四步：添加启动画面
直接从桌面启动的方式叫做standalone 模式，可以通过window. Standalone来进行判断是浏览器打开还是从桌面icon启动。
如果不进行启动画面设置，系统将会在在桌面启动webapp的时候展示一个白屏或者是上次从桌面启动的最后一个画面的截图。
启动画面注意事项：
1、启动画面有retina与非retina屏幕之分、横竖屏之分。格式必须为png
2、Iphone和ipod系统只支持竖屏启动（横屏时桌面不会跟着旋转），ipad横竖屏都可以。
3、启动画面是像素敏感的，也就是说像素不对的不能显示。
4、iPad必须是1024*748（横屏）、768*1004（竖屏），retina屏是2048*1496和1536*2008（ipad实测横屏启动设置无效）；
5、iPhone/iPod touch的只有竖屏，必须是320*460，对应Retina屏幕是640*920。
6、拿iphone举例来说retina屏幕对于320*460和640*920都可以正常识别和使用，但是非retina屏的不能识别大尺寸的图；同理与ipod和ipad
下面举例说明，结合media query来实现屏幕横竖屏和不同尺寸的启动画面的设置。
Iphone和ipod：
<link rel="apple-touch-startup-image" href="startup.png" media="screen and (max-device-width:320)" />
<link rel="apple-touch-startup-image" href="startup@2x.jpg" />

Ipad的竖屏可以如下设置，media里的orientation:landscape为必填项，否则横屏启动画面无效：
<link rel="apple-touch-startup-image" href="/st/assets/img/icon.png" media="screen and (min-device-width: 481px) and (max-device-width: 1024px) and (orientation:landscape)" />


经过这样四个步骤，添加到主屏功能就完备了。

   iOS 支持程度:
1、  iOS 1.1.3之后的版本有添加到主屏功能。
2、  iOS 2.0之后直接用名为apple-touch-icon-precomposed.png  的图片设置主屏图片效果。
3、  iOS 4.2 之后支持多个icon的设置，来支持多个不同设备的图片设置

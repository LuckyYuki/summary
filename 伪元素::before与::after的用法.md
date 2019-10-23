# 伪元素::before与::after的用法

## :before和::before是什么区别

- 相同点
  - 都可以用来表示伪类对象，用来设置对象前的内容
  - :befor和::before写法是等效的
- 不同点
  - :befor是Css2的写法，::before是Css3的写法
  - :before的兼容性要比::before好 ，不过在H5开发中建议使用::before比较好
- 加分项
  - 伪类对象要配合content属性一起使用
  - 伪类对象不会出现在DOM中，所以不能通过js来操作，仅仅是在 CSS 渲染层加入
  - 伪类对象的特效通常要使用:hover伪类样式来激活

  .test:hover::before { /* 这时animation和transition才生效 */ }  

## 与普通元素一样可以给其添加样式

```css
.del{ font-size: 20px;}
.del::before{ content: ""; display: inline-block; width: 20px; height: 25px; margin-right: 2px; vertical-align: middle; 
        background: url("imgs/delete.png") no-repeat center; background-size: 100%;}
.del span{ vertical-align: middle;}
```

## 使用::after伪元素解决经典清除浮动的问题

如果你网站还需要兼容IE8，那还是用:after吧，::after不兼容。

```css
.clearfix::after{ display:block; clear:both; content:""; overflow:hidden; height:0; }
```

## 在元素中插入文本

```css
.up:after{ content: '↑'; color: #f00;}
.down:after{ content: '↓'; color: #0f0;}
```

## 在元素中插入图像

```css
.del{ font-size: 20px;}
.del::before{ content: url("imgs/delete.png"); display: inline-block; margin-right: 2px; vertical-align: middle; }
.del span{ vertical-align: middle;}
```

需要非常注意的是，使用这种方式插入的图片并不能通过控制伪元素的大小来改变图片的大小，只能引入固定大小的图片（这个略坑啊...），所以个人觉得最好还是老老实实用背景图片比较好。

## 插入连续项目编号

```css
ul li{ list-style: none; counter-increment: number;}   
/* number相当于是个变量，随便取名就好，在伪元素中调用 */
ul li::before{ content: counter(number)"."; font-weight: bold;}  
/* 注意这里不同于JS，counter(number)与"."之间不需要加任何东西，直接连接就好 */
```

那我如果不想要阿拉伯数字，我就想用中文数字可以么？

```css
ul li{ list-style: none; counter-increment: number;}  
ul li::before{ content: counter(number,cjk-ideographic)"、"; font-weight: bold;}
```

cjk-ideographic 说明：参考CSS中 list-style-type 属性。

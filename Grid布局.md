## 关于 grid-template-areas

https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-template-areas


```html
<section id="page">
  <header>Header</header>
  <nav>Navigation</nav>
  <main>Main area</main>
  <footer>Footer</footer>
</section>
```

```css
#page {
  display: grid; /* 1.设置display为grid */
  width: 100%;
  height: 250px;
  grid-template-areas: "head head"
                       "nav  main"
                       "nav  foot"; /* 2.区域划分 当前为 三行 两列 */
  grid-template-rows: 50px 1fr 30px; /* 3.各区域 宽高设置 */
  grid-template-columns: 150px 1fr; 
}

#page > header {
  grid-area: head; /* 4. 指定当前元素所在的区域位置, 从grid-template-areas选取值 */
  background-color: #8ca0ff;
}

#page > nav {
  grid-area: nav;
  background-color: #ffa08c;
}

#page > main {
  grid-area: main;
  background-color: #ffff64;
}

#page > footer {
  grid-area: foot;
  background-color: #8cffa0;
}
```

## grid 布局

http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html

### 基本概念

- 容器和项目
- 行和列
- 单元格
  - 正常情况下，n行和m列会产生n x m个单元格
- 网格线
  - 正常情况下，n行有n + 1根水平网格线，m列有m + 1根垂直网格线

### 容器属性

- display: grid指定一个容器采用网格布局

默认情况下，容器元素都是块级元素，但也可以设成行内元素：display: inline-grid

> 注意，设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-*等设置都将失效。

- grid-template-columns属性定义每一列的列宽
- grid-template-rows属性定义每一行的行高。

也可以用百分比。

有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用auto-fill关键字表示自动填充。

**fr 关键字**

为了方便表示比例关系，网格布局提供了fr关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为1fr和2fr，就表示后者是前者的两倍。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}

.container {
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}

.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}

.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

- minmax()

minmax()函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

上面代码中，minmax(100px, 1fr)表示列宽不小于100px，不大于1fr。

**两栏式布局只需要一行代码**


```css
.wrapper {
  display: grid;
  grid-template-columns: 70% 30%;
}
```

- grid-row-gap用于设置行间距，grid-column-gap用于设置列间距。

grid-gap 属性是 grid-column-gap 和 grid-row-gap 的合并简写形式，语法如下。

```css
grid-gap: <grid-row-gap> <grid-column-gap>;
```

**根据最新标准，上面三个属性名的grid-前缀已经删除，grid-column-gap和grid-row-gap写成column-gap和row-gap，grid-gap写成gap。**

- grid-template-areas

grid-template-areas属性用于定义区域

- grid-auto-flow

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行

**这个顺序由grid-auto-flow属性决定，默认值是row，即"先行后列"。也可以将它设成column，变成"先列后行"。**

- justify-items 属性设置单元格内容的水平位置（左中右）
- align-items 属性设置单元格内容的垂直位置（上中下）。

place-items 属性是align-items属性和justify-items属性的合并简写形式。

```css
place-items: <align-items> <justify-items>;
/* 下面是一个例子。 */
place-items: start end;
/* 如果省略第二个值，则浏览器认为与第一个值相等。 */
```

- justify-content属性是整个内容区域在容器里面的水平位置（左中右）
- align-content属性是整个内容区域的垂直位置（上中下）。

place-content 属性是align-content属性和justify-content属性的合并简写形式。

```css
place-content: <align-content> <justify-content>
/* 下面是一个例子。 */

place-content: space-around space-evenly;
/* 如果省略第二个值，浏览器就会假定第二个值等于第一个值。 */
```

有时候，一些项目的指定位置，在现有网格的外部。比如网格只有3列，但是某一个项目指定在第5行。这时，浏览器会自动生成多余的网格，以便放置项目。
**grid-auto-columns 属性和grid-auto-rows 属性用来设置，浏览器自动创建的多余网格的列宽和行高。**它们的写法与grid-template-columns和grid-template-rows完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

### 项目属性

项目的位置是可以指定的，具体方法就是指定项目的四个边框，分别定位在哪根网格线。

- grid-column-start属性：左边框所在的垂直网格线
- grid-column-end属性：右边框所在的垂直网格线
- grid-row-start属性：上边框所在的水平网格线
- grid-row-end属性：下边框所在的水平网格线

这四个属性的值还可以使用span关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。


```css
.item-1 {
  grid-column-start: span 2;
}
/* 上面代码表示，1号项目的左边框距离右边框跨越2个网格 */
```

- grid-column属性是grid-column-start和grid-column-end的合并简写形式
- grid-row属性是grid-row-start属性和grid-row-end的合并简写形式。


```css
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}
```

上面代码中，项目item-1占据第一行，从第一根列线到第三根列线。

- grid-area属性指定项目放在哪一个区域。


```css
.item-1 {
  grid-area: e;
}
```

grid-area属性还可用作grid-row-start、grid-column-start、grid-row-end、grid-column-end的合并简写形式，直接指定项目的位置。


```css
.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```

- justify-self属性设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致，但只作用于单个项目。

- align-self属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目。

place-self属性是align-self属性和justify-self属性的合并简写形式。

```css
place-self: <align-self> <justify-self>;
```

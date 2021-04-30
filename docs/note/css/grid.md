# CSS Grid 网格布局教程

[[toc]]

## 一、概述

Grid 布局与 Flex 布局有一定的相似性，都可以指定容器内部多个项目的位置。

Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是一维布局。Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是二维布局。Grid 布局远比 Flex 布局强大。

## 二、基本概念

### 1. 容器和项目

采用网格布局的区域，称为"容器"（container）。容器内部采用网格定位的子元素，称为"项目"（item）。

> 项目只能是容器的顶层子元素，不包含项目的子元素。Grid 布局只对项目生效。

### 2. 行和列

容器里面的水平区域称为"行"（row），垂直区域称为"列"（column）。

### 3. 单元格

行和列的交叉区域，称为"单元格"（cell）。

正常情况下，n 行和 m 列会产生 n x m 个单元格。

### 4. 网格线

划分网格的线，称为"网格线"（grid line）。水平网格线划分出行，垂直网格线划分出列。

正常情况下，n 行有 n + 1 根水平网格线，m 列有 m + 1 根垂直网格线。

## 三、容器属性

> Grid 布局的属性分成两类。一类定义在容器上面，称为容器属性；另一类定义在项目上面，称为项目属性。

### 1. display 属性

`display: grid` 指定一个容器采用网格布局。

默认情况下，容器元素都是块级元素，但也可以设成行内元素：`display: inline-grid`

> 设为网格布局以后，容器子元素（项目）的 float、display: inline-block、display: table-cell、vertical-align 和 column-\*等设置都将失效。

### 2. grid-template-columns、grid-template-rows 属性

grid-template-columns 属性定义每一列的列宽，grid-template-rows 属性定义每一行的行高。

```
// 三行三列的网格，列宽和行高都是100px。
// 绝对单位
grid-template-columns: 100px 100px 100px;
grid-template-rows: 100px 100px 100px;

// 百分比
grid-template-columns: 33.33% 33.33% 33.33%;
grid-template-rows: 33.33% 33.33% 33.33%;
```

#### (1) repeat()

简化重复的值，第一个参数是重复的次数，第二个参数是所要重复的值。

```
// 三行三列，每行每列各占33.33%。
grid-template-columns: repeat(3, 33.33%);
grid-template-rows: repeat(3, 33.33%);

// 重复某种模式
// 第一列和第四列的宽度为100px，第二列和第五列为20px，第三列和第六列为80px。
grid-template-columns: repeat(2, 100px 20px 80px);
```

### （2） auto-fill 关键字

单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用 auto-fill 关键字表示自动填充：`grid-template-columns: repeat(auto-fill, 100px)`

### （3） fr 关键字

为了方便表示比例关系，网格布局提供了 fr 关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为 1fr 和 2fr，就表示后者是前者的两倍：`grid-template-columns: 1fr 1fr` , `grid-template-columns: 150px 1fr 2fr`

### （4） minmax()

产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。
`grid-template-columns: 1fr 1fr minmax(100px, 1fr)`
`minmax(100px, 1fr)` 表示列宽不小于 100px，不大于 1fr。

### （5） auto 关键字

表示由浏览器自己决定长度：`grid-template-columns: 100px auto 100px`

### （6） 网格线的名称

`grid-template-columns` 属性和 `grid-template-rows` 属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用：

```
grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
```

> 上面代码指定网格布局为 3 行 x 3 列，因此有 4 根垂直网格线和 4 根水平网格线。方括号里面依次是这八根线的名字。

网格布局允许同一根线有多个名字，比如[fifth-line row-5]。

### 3. grid-row-gap，grid-column-gap，grid-gap 属性

grid-row-gap 属性设置行与行的间隔（行间距），grid-column-gap 属性设置列与列的间隔（列间距）。

```
grid-row-gap: 20px;
grid-column-gap: 20px;
```

grid-gap 属性是 grid-column-gap 和 grid-row-gap 的合并简写形式：

```
grid-gap: <grid-row-gap> <grid-column-gap>;
grid-gap: 20px 20px;
```

如果 grid-gap 省略了第二个值，浏览器认为第二个值等于第一个值。

> 根据最新标准，上面三个属性名的 grid-前缀已经删除，grid-column-gap 和 grid-row-gap 写成 column-gap 和 row-gap，grid-gap 写成 gap。

### 4. grid-template-areas 属性

网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。grid-template-areas 属性用于定义区域。

```
// 3*3 单元格
grid-template-columns: 100px 100px 100px;
grid-template-rows: 100px 100px 100px;

// 划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。
grid-template-areas: 'a b c'
                      'd e f'
                      'g h i';

// 多个单元格合并成一个区域，将9个单元格分成a、b、c三个区域。
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';

// 顶部是页眉区域header，底部是页脚区域footer，中间部分则为main和sidebar。
grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";

// 某些区域不需要利用，则使用"点"（.）表示。
grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';
```

### 5. grid-auto-flow 属性

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行。这个顺序由 `grid-auto-flow` 属性决定，默认值是 row，即"先行后列"。也可以将它设成 column，变成"先列后行"。

```
grid-auto-flow: column;
```

还可以设成 `row dense` 和 `column dense` 。这两个值主要用于，某些项目指定位置以后，剩下的项目怎么自动放置。
设为 `row dense` ，表示"先行后列"，并且尽可能紧密填满，尽量不出现空格。
改为 `column dense` ，表示"先列后行"，并且尽量填满空格。

### 6. justify-items、align-items、place-items 属性

justify-items 属性设置单元格内容的水平位置（左中右），align-items 属性设置单元格内容的垂直位置（上中下）。

```
justify-items: start | end | center | stretch;
align-items: start | end | center | stretch;
```

- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）
  place-items 属性是 align-items 属性和 justify-items 属性的合并简写形式。

### 7. justify-content、align-content、place-content 属性

justify-content 属性是整个内容区域在容器里面的水平位置（左中右），align-content 属性是整个内容区域的垂直位置（上中下）。

```
justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
align-content: start | end | center | stretch | space-around | space-between | space-evenly;
```

- start - 对齐容器的起始边框。
- end - 对齐容器的结束边框。
- center - 容器内部居中。
- stretch - 项目大小没有指定时，拉伸占据整个网格容器。
- space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
- space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。
- space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

place-content 属性是 align-content 属性和 justify-content 属性的合并简写形式。

```
place-content: <align-content> <justify-content>
```

### 8. grid-auto-columns、grid-auto-rows 属性

有时候，一些项目的指定位置，在现有网格的外部。比如网格只有 3 列，但是某一个项目指定在第 5 行。这时，浏览器会自动生成多余的网格，以便放置项目。

grid-auto-columns 属性和 grid-auto-rows 属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与 grid-template-columns 和 grid-template-rows 完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

```
grid-auto-rows: 50px;
```

### 9. grid-template、grid 属性

grid-template 属性是 grid-template-columns、grid-template-rows 和 grid-template-areas 这三个属性的合并简写形式。

grid 属性是 grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow 这六个属性的合并简写形式。

## 四、项目属性

> 下面这些属性定义在项目上面。

### 1. grid-column-start、grid-column-end、grid-row-start、grid-row-end 属性

项目的位置是可以指定的，具体方法就是指定项目的四个边框，分别定位在哪根网格线。

- grid-column-start 属性：左边框所在的垂直网格线
- grid-column-end 属性：右边框所在的垂直网格线
- grid-row-start 属性：上边框所在的水平网格线
- grid-row-end 属性：下边框所在的水平网格线

没有指定上下边框，所以会采用默认位置，没有指定位置，由浏览器自动布局。

### 2. grid-column、grid-row 属性

grid-column 属性是 grid-column-start 和 grid-column-end 的合并简写形式，grid-row 属性是 grid-row-start 属性和 grid-row-end 的合并简写形式。

```
grid-column: <start-line> / <end-line>;
grid-row: <start-line> / <end-line>;
```

### 3. grid-area 属性

grid-area 属性指定项目放在哪一个区域。

```
grid-area: e;

grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
```

### 4. justify-self、align-self、place-self 属性

justify-self 属性设置单元格内容的水平位置（左中右），跟 justify-items 属性的用法完全一致，但只作用于单个项目。

align-self 属性设置单元格内容的垂直位置（上中下），跟 align-items 属性的用法完全一致，也是只作用于单个项目。

```
justify-self: start | end | center | stretch;
align-self: start | end | center | stretch;
```

- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）。

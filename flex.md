# flex基本操作

```
display: -webkit-flex; /* Webkit 内核的浏览器 */
display: flex;

display: inline-flex;

三、容器的属性

以下6个属性设置在容器上。

1、flex-direction属性决定主轴的方向
flex-direction: row | row-reverse | column | column-reverse;

flex-wrap属性定义，如果一条轴线排不下，如何换行。
flex-wrap: nowrap | wrap | wrap-reverse;

flex-flow: <flex-direction> || <flex-wrap>;

justify-content属性定义了项目在主轴上的对齐方式。
justify-content: flex-start | flex-end | center | space-between | space-around;

align-items属性定义项目在交叉轴上如何对齐。
align-items: flex-start | flex-end | center | baseline | stretch;

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
align-content: flex-start | flex-end | center | space-between | space-around | stretch;

四、项目的属性
order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
order: <integer>;

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
flex-grow: <number>; /* default 0 */

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
flex-shrink: <number>; /* default 1 */

flex-basis属性定义了在分配多余空间之前
flex-basis: <length> | auto; /* default auto */
flex-basis:33.33%

flex 默认  0 1 auto
flex：auto  1 1 auto
flex：none 0 0 auto

align-self属性允许单个项目有与其他项目不一样的对齐方式，
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```
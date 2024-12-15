
# 1、概述


栅格布局是一种通用的辅助定位工具，对移动设备的界面设计有较好的借鉴作用。主要优势包括：


1. 提供可循的规律：栅格布局可以为布局提供规律性的结构，解决多尺寸多设备的动态布局问题。通过将页面划分为等宽的列数和行数，可以方便地对页面元素进行定位和排版。
2. 统一的定位标注：栅格布局可以为系统提供一种统一的定位标注，保证不同设备上各个模块的布局一致性。这可以减少设计和开发的复杂度，提高工作效率。
3. 灵活的间距调整方法：栅格布局可以提供一种灵活的间距调整方法，满足特殊场景布局调整的需求。通过调整列与列之间和行与行之间的间距，可以控制整个页面的排版效果。
4. 自动换行和自适应：栅格布局可以完成一对多布局的自动换行和自适应。当页面元素的数量超出了一行或一列的容量时，他们会自动换到下一行或下一列，并且在不同的设备上自适应排版，使得页面布局更加灵活和适应性强。


GridRow为栅格容器组件，需与栅格子组件GridCol在栅格布局场景中联合使用。


# 2、栅格容器GridRow


### 👉🏻 2\.1、栅格系统断点


栅格系统以设备的水平宽度（屏幕密度像素值，单位vp）作为断点依据，定义设备的宽度类型，形成了一套断点规则。开发者可根据需求在不同的断点区间实现不同的页面布局效果。


栅格系统默认断点将设备宽度分为xs、sm、md、lg四类，尺寸范围如下：


![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/4b64e82c72e84159a1480f8823f0894a~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=1MHt9VY8XQ5ofnTTSkrgJuLGQyE%3D)


在GridRow栅格组件中，允许开发者使用breakpoints自定义修改断点的取值范围，最多支持6个断点，除了默认的四个断点外，还可以启用xl，xxl两个断点，支持六种不同尺寸（xs, sm, md, lg, xl, xxl）设备的布局设置。


![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/52d9482763f94e5a8e1129b406b98f25~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=1ewfmv6p7wtFHUfG3VFsr5auqdA%3D)


* 针对断点位置，开发者根据实际使用场景，通过一个单调递增数组设置。由于breakpoints最多支持六个断点，单调递增数组长度最大为5



```
breakpoints: {value: ['100vp', '200vp']}

```

上述代码表示启用xs、sm、md共3个断点，小于100vp为xs，100vp\-200vp为sm，大于200vp为md。



```
breakpoints: {value: ['320vp', '520vp', '840vp', '1080vp']}

```

上述代码表示启用xs、sm、md、lg、xl共5个断点，小于320vp为xs，320vp\-520vp为sm，520vp\-840vp为md，840vp\-1080vp为lg，大于1080vp为xl。


* 栅格系统通过监听窗口或容器的尺寸变化进行断点，通过reference设置断点切换参考物。考虑到应用可能以非全屏窗口的形式显示，以应用窗口宽度为参照物更为通用。


例如，使用栅格的默认列数12列，通过断点设置将应用宽度分成六个区间，在各区间中，每个栅格子元素占用的列数均不同。



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
...
GridRow({
  breakpoints: {
    value: ['200vp', '300vp', '400vp', '500vp', '600vp'],
    reference: BreakpointsReference.WindowSize
  }
}) {
   ForEach(this.bgColors, (color, index) => {
     GridCol({
       span: {
         xs: 2, // 在最小宽度类型设备上，栅格子组件占据的栅格容器2列。
         sm: 3, // 在小宽度类型设备上，栅格子组件占据的栅格容器3列。
         md: 4, // 在中等宽度类型设备上，栅格子组件占据的栅格容器4列。
         lg: 6, // 在大宽度类型设备上，栅格子组件占据的栅格容器6列。
         xl: 8, // 在特大宽度类型设备上，栅格子组件占据的栅格容器8列。
         xxl: 12 // 在超大宽度类型设备上，栅格子组件占据的栅格容器12列。
       }
     }) {
       Row() {
         Text(`${index}`)
       }.width("100%").height('50vp')
     }.backgroundColor(color)
   })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/08f57ca35d1240ad86d0098659f3db90~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=kKl8VtqOECy7koi80ZrSNOLrTeg%3D)


### 👉🏻 2\.2、布局的总列数


GridRow中通过columns设置栅格布局的总列数。


* columns默认值为12，即在未设置columns时，任何断点下，栅格布局被分成12列



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown,Color.Red, Color.Orange, Color.Yellow, Color.Green];
...
GridRow() {
  ForEach(this.bgColors, (item, index) => {
    GridCol() {
      Row() {
        Text(`${index + 1}`)
      }.width('100%').height('50')
    }.backgroundColor(item)
  })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/3dea285849484051b5f5d620fcecce8a~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=QxK1ZnTycuWYPcmk0cx%2BGBWZ4ew%3D)


* 当columns为自定义值，栅格布局在任何尺寸设备下都被分为columns列。下面分别设置栅格布局列数为4和8，子元素默认占一列，效果如下



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
@State currentBp: string = 'unknown';
...
Row() {
  GridRow({ columns: 4 }) {
    ForEach(this.bgColors, (item, index) => {
      GridCol() {
        Row() {
          Text(`${index + 1}`)
        }.width('100%').height('50')
      }.backgroundColor(item)
    })
  }
  .width('100%').height('100%')
  .onBreakpointChange((breakpoint) => {
    this.currentBp = breakpoint
  })
}
.height(160)
.border({ color: Color.Blue, width: 2 })
.width('90%')

Row() {
  GridRow({ columns: 8 }) {
    ForEach(this.bgColors, (item, index) => {
      GridCol() {
        Row() {
          Text(`${index + 1}`)
        }.width('100%').height('50')
      }.backgroundColor(item)
    })
  }
  .width('100%').height('100%')
  .onBreakpointChange((breakpoint) => {
    this.currentBp = breakpoint
  })
}
.height(160)
.border({ color: Color.Blue, width: 2 })
.width('90%')

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/81fb41ab4ade47b8bb3216b995fe4738~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=Yu0cwJuyYPgYC4OkF8PaycLF3nY%3D)


* **当columns类型为GridRowColumnOption时，支持下面六种不同尺寸（xs, sm, md, lg, xl, xxl）设备的总列数设置，各个尺寸下数值可不同。**



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown]
GridRow({ columns: { sm: 4, md: 8 }, breakpoints: { value: ['200vp', '300vp', '400vp', '500vp', '600vp'] } }) {
  ForEach(this.bgColors, (item, index) => {
    GridCol() {
      Row() {
        Text(`${index + 1}`)
      }.width('100%').height('50')
    }.backgroundColor(item)
  })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/8cc38b98ce8d416fb8ebf31e67bd8a9a~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=NObK51ySUd%2FcCsIlzEANh23OCIg%3D)


### 👉🏻 2\.3、排列方向


栅格布局中，可以通过设置GridRow的direction属性来指定栅格子组件在栅格容器中的排列方向。该属性可以设置为GridRowDirection.Row（从左往右排列）或GridRowDirection.RowReverse（从右往左排列），以满足不同的布局需求。通过合理的direction属性设置，可以使得页面布局更加灵活和符合设计要求。


* 子组件默认从左往右排列



```
GridRow({ direction: GridRowDirection.Row }){}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/7248daed6e1a4532b0e246022b4aa8ef~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=0PHb05%2B71rrbXCkGe1rWSyaIkco%3D)


* 子组件从右往左排列



```
GridRow({ direction: GridRowDirection.RowReverse }){}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/21ce5fd810a746e4b713c88dc46525fa~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=k4N8TAiHyfIT2vmUx06pa1G%2FH84%3D)


### 👉🏻 2\.4、子组件间距


GridRow中通过gutter属性设置子元素在水平和垂直方向的间距。


* 当gutter类型为number时，同时设置栅格子组件间水平和垂直方向边距且相等。下例中，设置子组件水平与垂直方向距离相邻元素的间距为10。



```
 GridRow({ gutter: 10 }){}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/04de41e24d074f1d8ae2f53bf55d8486~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=JSj7h69HSWi4hs5g3VEG%2FCdfZ20%3D)


* 当gutter类型为GutterOption时，单独设置栅格子组件水平垂直边距，x属性为水平方向间距，y为垂直方向间距



```
GridRow({ gutter: { x: 20, y: 50 } }){}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/e8f74bfdd0004df18d38a09008fc023c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=TV9DeFlrr5hPGvCK9wI%2Fj%2BKn3to%3D)


# 3、子组件GridCol


GridCol组件作为GridRow组件的子组件，通过给GridCol传参或者设置属性两种方式，设置span（占用列数），offset（偏移列数），order（元素序号）的值。


* 设置span。



```
GridCol({ span: 2 }){}
GridCol({ span: { xs: 1, sm: 2, md: 3, lg: 4 } }){}
GridCol(){}.span(2)
GridCol(){}.span({ xs: 1, sm: 2, md: 3, lg: 4 })

```

* 设置offset。



```
GridCol({ offset: 2 }){}
GridCol({ offset: { xs: 2, sm: 2, md: 2, lg: 2 } }){}
GridCol(){}.offset(2)
GridCol(){}.offset({ xs: 1, sm: 2, md: 3, lg: 4 })

```

* 设置order。



```
GridCol({ order: 2 }){}
GridCol({ order: { xs: 1, sm: 2, md: 3, lg: 4 } }){}
GridCol(){}.order(2)
GridCol(){}.order({ xs: 1, sm: 2, md: 3, lg: 4 })

```

## 👉🏻 3\.1、span


子组件占栅格布局的列数，决定了子组件的宽度，默认为1。


* 当类型为number时，子组件在所有尺寸设备下占用的列数相同



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
...
GridRow({ columns: 8 }) {
  ForEach(this.bgColors, (color, index) => {
    GridCol({ span: 2 }) {      
      Row() {
        Text(`${index}`)
      }.width('100%').height('50vp')          
    }
    .backgroundColor(color)
  })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/7b1ac8ef4880412abf7245f42b1254d3~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=MbfSqL0aGH10Ycr2jJkv6zsVmP8%3D)


* 当类型为GridColColumnOption时，支持六种不同尺寸（xs, sm, md, lg, xl, xxl）设备中子组件所占列数设置,各个尺寸下数值可不同



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
...
GridRow({ columns: 8 }) {
  ForEach(this.bgColors, (color, index) => {
    GridCol({ span: { xs: 1, sm: 2, md: 3, lg: 4 } }) {      
      Row() {
        Text(`${index}`)
      }.width('100%').height('50vp')          
    }
    .backgroundColor(color)
  })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/736beaa14796470ab948f93c0ccbb019~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=6cUwaDnnWND5f%2BIMNze3pR6yw9A%3D)


## 👉🏻 3\.2、offset


栅格子组件相对于前一个子组件的偏移列数，默认为0。


* 当类型为number时，子组件偏移相同列数。



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
...
GridRow() {
  ForEach(this.bgColors, (color, index) => {
    GridCol({ offset: 2 }) {      
      Row() {
        Text('' + index)
      }.width('100%').height('50vp')          
    }
    .backgroundColor(color)
  })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/8b9b52d8149c46bfa25e9737cedefb11~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=fmVMFGqaydpaVK2Wf2UAh%2BGHlKk%3D)


栅格默认分成12列，每一个子组件默认占1列，偏移2列，每个子组件及间距共占3列，一行放四个子组件。


* 当类型为GridColColumnOption时，支持六种不同尺寸（xs, sm, md, lg, xl, xxl）设备中子组件所占列数设置,各个尺寸下数值可不同。



```
@State bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
...

GridRow() {
  ForEach(this.bgColors, (color, index) => {
    GridCol({ offset: { xs: 1, sm: 2, md: 3, lg: 4 } }) {      
      Row() {
        Text('' + index)
      }.width('100%').height('50vp')          
    }
    .backgroundColor(color)
  })
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/b613d54c3ef642249b3149e9ff386f64~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=FsXNPY7R9hcU6YajDI5Qej2IcQc%3D)


## 👉🏻 3\.3、order


栅格子组件的序号，决定子组件排列次序。当子组件不设置order或者设置相同的order, 子组件按照代码顺序展示。当子组件设置不同的order时，order较小的组件在前，较大的在后。


当子组件部分设置order，部分不设置order时，未设置order的子组件依次排序靠前，设置了order的子组件按照数值从小到大排列。


* 当类型为number时，子组件在任何尺寸下排序次序一致。



```
GridRow() {
  GridCol({ order: 4 }) {
    Row() {
      Text('1')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Red)
  GridCol({ order: 3 }) {
    Row() {
      Text('2')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Orange)
  GridCol({ order: 2 }) {
    Row() {
      Text('3')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Yellow)
  GridCol({ order: 1 }) {
    Row() {
      Text('4')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Green)
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/89bd75e80e0842fa9372894c6d1c4690~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=OBE5VbX4KI6vbA9IC%2Bg0JqIxswE%3D)


* 当类型为GridColColumnOption时，支持六种不同尺寸（xs, sm, md, lg, xl, xxl）设备中子组件排序次序设置。在xs设备中，子组件排列顺序为1234；sm为2341，md为3412，lg为2431。



```
GridRow() {
  GridCol({ order: { xs:1, sm:5, md:3, lg:7}}) {
    Row() {
      Text('1')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Red)
  GridCol({ order: { xs:2, sm:2, md:6, lg:1} }) {
    Row() {
      Text('2')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Orange)
  GridCol({ order: { xs:3, sm:3, md:1, lg:6} }) {
    Row() {
      Text('3')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Yellow)
  GridCol({ order: { xs:4, sm:4, md:2, lg:5} }) {
    Row() {
      Text('4')
    }.width('100%').height('50vp')
  }.backgroundColor(Color.Green)
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/1806c13fb98a413a9466050cb6153830~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=n4T6mnz7zYXZIkPu33mmJ5lt2uU%3D)


# 4、栅格组件的嵌套使用


栅格组件也可以嵌套使用，完成一些复杂的布局。


以下示例中，栅格把整个空间分为12份。第一层GridRow嵌套GridCol，分为中间大区域以及“footer”区域。第二层GridRow嵌套GridCol，分为“left”和“right”区域。子组件空间按照上一层父组件的空间划分，粉色的区域是屏幕空间的12列，绿色和蓝色的区域是父组件GridCol的12列，依次进行空间的划分。



```
@Entry
@Component
struct GridRowExample {
  build() {
    GridRow() {
      GridCol({ span: { sm: 12 } }) {
        GridRow() {
          GridCol({ span: { sm: 2 } }) {
            Row() {
              Text('left').fontSize(24)
            }
            .justifyContent(FlexAlign.Center)
            .height('90%')
          }.backgroundColor('#ff41dbaa')

          GridCol({ span: { sm: 10 } }) {
            Row() {
              Text('right').fontSize(24)
            }
            .justifyContent(FlexAlign.Center)
            .height('90%')
          }.backgroundColor('#ff4168db')
        }
        .backgroundColor('#19000000')
        .height('100%')
      }

      GridCol({ span: { sm: 12 } }) {
        Row() {
          Text('footer').width('100%').textAlign(TextAlign.Center)
        }.width('100%').height('10%').backgroundColor(Color.Pink)
      }
    }.width('100%').height(300)
  }
}

```

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/2dd51c645048408a98bfde7a9e7bc38b~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg6bi_6JKZ6Ieq5Lmg5a6k:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzQzNzU1MTkxNjQyMzQ4MyJ9&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1734708206&x-orig-sign=DvL9z8I5xFFVGAZySKQro%2FvBnbw%3D)


综上所述，栅格组件提供了丰富的自定义能力，功能异常灵活和强大。只需要明确栅格在不同断点下的Columns、Margin、Gutter及span等参数，即可确定最终布局，无需关心具体的设备类型及设备状态（如横竖屏）等。


# 5、结语


后续还有网格布局，请持续关注“ArkTs布局入门06”


如果你也对鸿蒙开发感兴趣，加入“Harmony自习室”吧，点击下面的名片关注公众号。


 本博客参考[slower加速器](https://jisuanqi.org)。转载请注明出处！

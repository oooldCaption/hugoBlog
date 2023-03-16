---
title: "Flutter 渐变"
date: 2022-11-20T17:52:38+08:00
draft: false
tags: ['flutter']
categories: ['软件开发']
---

太久没写技术博客啦~~~~~~~


最近一直在用`fluter`做app,  前几天有个功能需要用渐变色来做一个文本. 

我回想起几年前被我们UI设计师支配的恐惧, 那个UI特别喜欢用渐变色跟圆角, 以至于天天跟CALayer之类的打交道.   

如果想要在flutter上实现渐变色就太简单了, 在`Container`容器中有一个修饰器`decoration`中 有一个 `gradient`属性,  你只要给这个属性设置值, 就可以对容器设置渐变色了. 
对修饰器有以下几种创建渐变色的方式:

* LinearGradient
* RadialGradient
* SweepGradient

我会在下面的内容分别对上面这 3 种实现渐变色的方法给出具体的实例和相关代码:

### LinearGradient
LinearGradient 是 Flutter 中实现线性渐变效果的类，它可以用来创建两个或多个颜色之间的线性渐变。下面是 LinearGradient 的一些适用场景和利弊：

**适用场景：**

需要实现颜色的线性渐变效果，例如从一个颜色平滑过渡到另一个颜色；可以在任何需要使用颜色的地方使用，例如背景、边框、文本等。

**利弊：**

优点：简单易用，可以创建平滑过渡的颜色效果，适用于各种需要渐变色的场景。

缺点：不适合实现复杂的渐变效果，例如径向渐变或多个颜色之间的复杂渐变。对于这种情况，可以考虑使用 RadialGradient 或 SweepGradient 等其他类型的渐变。

LinearGradient中也有很多其他的属性, 你可以自己尝试一下都有什么效果

* `begin`：定义渐变的起点位置，它是一个Alignment类型的对象。Alignment类型包含两个属性：x和y，分别表示水平和垂直方向上的偏移量。默认值是Alignment.centerLeft，表示从左到右进行渐变。

* `end`：定义渐变的终点位置，也是一个Alignment类型的对象。默认值是Alignment.centerRight，表示从右到左进行渐变。

* `colors`：定义渐变的颜色数组。渐变将从数组的第一个颜色过渡到最后一个颜色。至少需要提供两个颜色。

* `stops`：定义颜色数组中每个颜色的位置。每个位置的值必须在0.0到1.0之间。如果不提供stops属性，则渐变中每个颜色的位置将均匀分布。

* `tileMode`：定义渐变在超出定义范围时的重复方式。默认值是TileMode.clamp，表示渐变将延伸到起点和终点之外的区域，并使用最近的颜色。模式如下图所示: 

* ![](https://flutter.github.io/assets-for-api-docs/assets/dart-ui/tile_mode_clamp_linear.png)
![](https://flutter.github.io/assets-for-api-docs/assets/dart-ui/tile_mode_decal_linear.png)
![](https://flutter.github.io/assets-for-api-docs/assets/dart-ui/tile_mode_mirror_linear.png)
![](https://flutter.github.io/assets-for-api-docs/assets/dart-ui/tile_mode_repeated_linear.png)

LinearGradient 的简单应用, 代码 以及 图例 如下: 

```
 // LinearGradient
  _linear() {
    return Center(
      child: Container(
          height: 100,
          width: 100,
          decoration: const BoxDecoration(
            gradient: LinearGradient(
              begin: Alignment.centerLeft,
              end: Alignment.centerRight,
              colors: [Colors.blue, Colors.black38,Colors.green],
            ),
          ),
          child: Text("LinearGradient_渐变") 
          ),
    );
  }

```

![1678863878.png](https://img.52smile.vip/1678863878.png)

以上是LinearGradient的一些重要属性，您可以根据需要进行自定义和扩展，以创建更复杂和有趣的渐变效果。

### RadialGradient
  

RadialGradient 是 Flutter 中实现径向渐变效果的类，它可以用来创建从一个中心点向周围扩散的渐变效果。下面是 RadialGradient 的一些适用场景和利弊：

**适用场景**：

需要实现从一个中心点向周围扩散的渐变效果，例如圆形背景、圆形边框等；
可以在任何需要使用颜色的地方使用，例如背景、边框、文本等。
利弊：

**优点**：可以创建从一个中心点向周围扩散的渐变效果，适用于多种需要径向渐变的场景；

**缺点**：不适合实现复杂的渐变效果，例如多个颜色之间的复杂渐变。对于这种情况，可以考虑使用 LinearGradient 或 SweepGradient 等其他类型的渐变。

RadialGradient 属性列表以及说明:

`center`：定义渐变的中心点位置，它是一个Alignment类型的对象。Alignment类型包含两个属性：x和y，分别表示水平和垂直方向上的偏移量。默认值是Alignment.center，表示渐变从中心点开始。

`radius`：定义渐变的半径，它是一个浮点数，表示从中心点开始的半径。默认值是0.5。

`colors`：定义渐变的颜色数组。渐变将从数组的第一个颜色过渡到最后一个颜色。至少需要提供两个颜色。

`stops`：定义颜色数组中每个颜色的位置。每个位置的值必须在0.0到1.0之间。如果不提供stops属性，则渐变中每个颜色的位置将均匀分布。

`tileMode`：定义渐变在超出定义范围时的重复方式。默认值是TileMode.clamp，表示渐变将延伸到半径之外的区域，并使用最近的颜色。

RadialGradient  的简单应用, 代码 以及 图例 如下: 

```
  _radial() {
    return Center(
      child: Container(
          height: 100,
          width: 100,
          decoration: const BoxDecoration(
            gradient: RadialGradient(
              center: Alignment(0.0, 0.0),
              radius: 0.5,
              colors: [Colors.red, Colors.yellow, Colors.green],
              stops: [0.0, 0.5, 1.0],
              tileMode: TileMode.repeated,
            ),
          ),
          child: Text("RadialGradient 渐变")),
    );
  }

```
![1678865491.png](https://img.52smile.vip/1678865491.png)

在上面的代码中，我们创建了一个 `Container` 组件，并使用 `RadialGradient` 作为其背景。我们将渐变的中心点位置设置为`Alignment(0.0, 0.0)`，将半径设置为0.5，将颜色数组设置为`[Colors.red, Colors.yellow, Colors.green]`，并将停止位置数组设置为`[0.0, 0.5, 1.0]`。这意味着渐变将从红色开始，到黄色中间停止，然后到绿色结束。我们还将`tileMode`设置为`TileMode.repeated`，表示渐变将在超出定义范围时重复。


### SweepGradient
SweepGradient 是 Flutter 中实现扫描渐变效果的类，它可以用来创建在一个圆圈内均匀分布的颜色渐变效果。下面是 SweepGradient 的一些适用场景和利弊：

**适用场景**：

需要实现在一个圆圈内均匀分布的颜色渐变效果，例如圆形背景、圆形边框等；
可以在任何需要使用颜色的地方使用，例如背景、边框、文本等。
利弊：

**优点**：可以创建在一个圆圈内均匀分布的颜色渐变效果，适用于多种需要扫描渐变的场景；

**缺点**：不适合实现复杂的渐变效果，例如多个颜色之间的复杂渐变。对于这种情况，可以考虑使用 LinearGradient 或 RadialGradient 等其他类型的渐变。

SweepGradient 属性列表以及说明:

`center`：定义渐变的中心点位置，它是一个Alignment类型的对象。Alignment类型包含两个属性：x和y，分别表示水平和垂直方向上的偏移量。默认值是Alignment.center，表示渐变从中心点开始。

`startAngle`：定义渐变的起始角度，它是一个浮点数，表示从中心点开始的起始角度。默认值是0.0，表示从正右方开始扫描。

`endAngle`：定义渐变的结束角度，它是一个浮点数，表示从中心点开始的结束角度。默认值是2 * pi，表示扫描一周结束。

`colors`：定义渐变的颜色数组。渐变将从数组的第一个颜色过渡到最后一个颜色。至少需要提供两个颜色。

`stops`：定义颜色数组中每个颜色的位置。每个位置的值必须在0.0到1.0之间。如果不提供stops属性，则渐变中每个颜色的位置将均匀分布。

SweepGradient 的简单应用, 代码 以及 图例 如下:

```
  _sweepGradient(){
    return Center(
      child: Container(
          decoration: const BoxDecoration(
            gradient: SweepGradient(
              center: Alignment.center,
              startAngle: 0.0,
              endAngle: 2 * pi,
              colors: [Colors.red, Colors.yellow, Colors.green],
              stops: [0.0, 0.5, 1.0],
            ),
          ),
          child: Text("SweepGradient 渐变"))
    );

  }
```

![1678866078.png](https://img.52smile.vip/1678866078.png)

在上面的代码中，我们创建了一个Container组件，并使用SweepGradient作为其背景。我们将渐变的中心点位置设置为Alignment.center，将起始角度设置为0.0，将结束角度设置为2 * pi，将颜色数组设置为[Colors.red, Colors.yellow, Colors.green]，并将停止位置数组设置为[0.0, 0.5, 1.0]。这意味着渐变将从红色开始，到黄色中间停止，然后到绿色结束。

### 文本渐变

这个是经常会在苹果官网看到, 绚丽多彩的渐变文本. 我们也可以用 flutter 轻松实现: 

![1678866568.png](https://img.52smile.vip/1678866568.png)

```
  _colorfulText() {
    return ShaderMask(
      shaderCallback: (bounds) {
        return const LinearGradient(
          colors: [Colors.blue, Colors.red],
          stops: [0.0, 1.0],
          begin: Alignment.centerLeft,
          end: Alignment.centerRight,
        ).createShader(bounds);
      },
      child: const Text(
        "this is way",
        style: TextStyle(
          fontSize: 32.0,
          fontWeight: FontWeight.bold,
          color: Colors.white,
        ),
      ),
    );
  }

```


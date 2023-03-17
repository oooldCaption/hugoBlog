---
title: "使用Flutter创建樱花落下动画"
date: 2023-03-16T15:18:02+08:00
draft: 
tags: ['flutter']
categories: ['软件开发']
---

在本篇博客中，我们将学习如何使用 Flutter 制作一个樱花落下的动画效果。我们将实现一个简单的樱花落下动画，并加入微风效果以及淡入淡出效果。我们的最终目标是让樱花以不同的速度和初始位置落下，并在屏幕上按照一定的轨迹随微风飘动。
![1678950904.gif](https://img.52smile.vip/1678950904.gif)

开始之前
确保你已经安装了 Flutter 开发环境。本教程适用于具有一定 Flutter 基础知识的开发者。

### 创建动画
首先，我们需要创建一个 StatefulWidget，因为我们需要使用动画控制器。接下来，我们在 State 类中使用 SingleTickerProviderStateMixin 来创建 AnimationController。

```
class FallingLeavesAnimation extends StatefulWidget {
  @override
  _FallingLeavesAnimationState createState() => _FallingLeavesAnimationState();
}

class _FallingLeavesAnimationState extends State<FallingLeavesAnimation>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;
  final _leaves = List.generate(20, (index) => Leaf());

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 15),
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
```

我们创建了一个 _controller，并设置动画持续时间为 15 秒。然后调用 repeat() 方法，让动画循环播放。

### 美化动画
接下来，我们创建一个 Leaf 类来生成随机的初始位置、速度和旋转角度。这将使动画看起来更自然。
```

class Leaf {
  final double startX;
  final double startY;
  final double speedX;
  final double speedY;
  final double rotation;

  Leaf()
      : startX = Random().nextDouble() * 300,
        startY = -Random().nextDouble() * 300,
        speedX = 20 + Random().nextDouble() * 50,
        speedY = 50 + Random().nextDouble() * 100,
        rotation = Random().nextDouble() * 4 * pi;
}
```

### 执行动画并更新
现在，我们将使用 AnimatedBuilder 在 build 方法中创建动画效果。在每次 build 方法调用时，动画进度会被更新，根据动画进度计算樱花的位置、旋转角度和透明度。

```
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('Falling Leaves Animation with Breeze'),
    ),
    body: Stack(
      children: [
        for (final leaf in _leaves)
          AnimatedBuilder(
            animation: _controller,
            builder: (context, child) {
              final progress = _controller.value;
              final screenWidth =MediaQuery.of(context).size.width;
          final screenHeight = MediaQuery.of(context).size.height;
          final breeze = 50 * sin(2 * pi * progress);
          final leafX = leaf.startX + leaf.speedX * progress + breeze;
          final leafY = leaf.startY +
              leaf.speedY * progress +
              0.5 * 9.8 * pow(progress, 2);
          final rotation = leaf.rotation * progress;
          final opacity = (1 - progress) * (1 - progress);
          return Positioned(
            left: leafX,
            top: leafY % screenHeight,
            child: Transform.rotate(
              angle: rotation,
              child: Opacity(
                  opacity: opacity,
                child: SvgPicture.asset(
                  'assets/cherry_blossom.svg',
                  width: 25,
                  height: 25,
                ),
              ),
            ),
          );
        },
      ),
  ],
),
);
}
```

我们使用了 `Stack` 组件来确保樱花动画可以覆盖在其他组件上。然后，我们使用 `for` 循环遍历 `_leaves` 列表中的每个 `Leaf` 对象，并使用 `AnimatedBuilder` 更新动画效果。

在 `builder` 函数中，我们首先获取动画的进度值 `_controller.value`，然后计算屏幕的宽度和高度。我们使用正弦函数创建一个微风效果，使樱花在水平方向上随风飘动。接着，我们根据 `Leaf` 对象的初始位置和速度以及动画进度来计算樱花的当前位置和旋转角度。

为了使樱花在落下过程中逐渐消失，我们计算透明度值 `opacity`。最后，我们使用 `Positioned` 组件来设置樱花的位置，并使用 `Transform.rotate` 组件来设置旋转角度。为了实现淡入淡出效果，我们使用 `Opacity` 组件来设置樱花的透明度。

### 代码地址
相关的代码实现你可以在这里找到: [git_repo](https://github.com/oooldCaption/flutter_blog/blob/main/lib/2023_03/leaf_fall_anim.dart)

## 总结
在本教程中，我们学习了如何使用 Flutter 制作一个樱花落下的动画效果，包括微风效果和淡入淡出效果。我们了解了如何使用 `AnimationController` 控制动画进度，以及如何使用 `AnimatedBuilder` 更新动画效果。希望本教程对你有所帮助，祝你在 Flutter 开发中取得更多进步！


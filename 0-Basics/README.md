# Basics

介绍 THREE 关键结构

```
Renderer
Camera
Scene
  Mesh
    Geometry
    Material
      Textrue
  Object3D
  Group
  Light
```

Renderer

0. CSSRenderer
1. CanvasRenderer
2. WebGL1Renderer & WebGL2Renderer
3. WebGPURenderer

简答的盒子的显示 Demo

# Responsive Design

响应式，主要是根据窗口变化，或者 canvas 窗口的变化，更新一下值

```js
camera.aspect = canvas.clientWidth / canvas.clientHeight;
camera.updateProjectionMatrix();
renderer.setSize(width, height, false);
```

**强烈不建议这样**

```js
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(width, height, false);
```

推荐

```js
renderer.setPixelRatio(width * devicePixelRatio, height * height, false);
```

第二章方法从客观上来说更好。为什么？因为我拿到了我想要的。 在使用 three.js 时有很多种情况下我们需要知道 canvas 的绘图缓冲区的确切尺寸。 比如制作后期处理滤镜或者我们在操作着色器需要访问 gl_FragCoord 变量，如果我们截屏或者给 GPU 读取像素，绘制到二维的 canvas 等等。 通过我们自己处理我们会一直知道使用的尺寸是不是我们需要的。 幕后并没有什么特殊的魔法发生。

剩下两篇，讲了前端基础，略过了

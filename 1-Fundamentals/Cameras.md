# Cameras

0. cube 正方体
1. cone 圆锥
2. sphere 球体
3. cylinder 圆柱
4. frustum 锥台（四棱锥切掉尖）

### PerspectiveCamera

0. near 近裁剪平面
1. far 远裁剪平面
2. aspect 宽高比
3. fov 上下面的夹角

near 太小，比如 0.00001，会由于没有足够精度，导致深度判断有误差

解决办法是设置`logarithmicDepthBuffer`
但是有兼容性问题，大部分桌面 GPU 支持，但是移动端 GPU 基本不支持
并且性能更慢，所以还是根据使用场景设置 near 和 far

```js
const renderer = new THREE.WebGLRenderer({
  canvas,
  logarithmicDepthBuffer: true,
});
```

### OrthographicCamera

```js
camera.left = -canvas.width / 2;
camera.right = canvas.width / 2;
camera.top = canvas.height / 2;
camera.bottom = -canvas.height / 2;
camera.near = -1;
camera.far = 1;
camera.zoom = 1;
```

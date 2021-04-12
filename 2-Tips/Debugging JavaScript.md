# Debugging JavaScript

如何打开调试工具

如何查看日志

clearing logger，每一帧都专门显示日志的区域，下一帧清空所有日志

注意 NaN

把 rFA 放到逻辑代码后面

```js
function render() {
  // -- do stuff --

  renderer.render(scene, camera);

  requestAnimationFrame(render);
}
requestAnimationFrame(render);
```

放到底部的好处是，有问题就停止渲染

The biggest reason is it means your code will stop if you have an error. Putting requestAnimationFrame at the top means your code will keep running even if you have an error since you already requested another frame. IMO it's better to find those errors than to ignore them. They could easily be the reason something is not appearing as you expect it to but unless your code stops you might not even notice.

IMO(In My Opinion) 依我所见

检查数据单位，比如弧度角度

使用 MeshBasicMaterial 排除光照阴影问题

Check your near and far settings for your camera

Check your scene is in front of the camera

Put something in front of the camera

慢慢把场景搭建依赖

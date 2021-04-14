# Taking A Screenshot of the Canvas

webgl 的 canvas 通过 canvas.toBlob 大部分情况会得到空白的图片，因为浏览器会情况 canvas 的 drawing buffer
The issue is that for performance and compatibility reasons, by default the browser will clear a WebGL canvas's drawing buffer after you've drawn to it.

解决办法是在截取之前 render 一次
The solution is to call your rendering code just before capturing.

```js
const elem = document.querySelector('#screenshot');
elem.addEventListener('click', () => {
  render(); // 渲染一次
  canvas.toBlob(blob => {
    saveBlob(blob, `screencapture-${canvas.width}x${canvas.height}.png`);
  });
});

const saveBlob = (function () {
  const a = document.createElement('a');
  document.body.appendChild(a);
  a.style.display = 'none';
  return function saveData(blob, fileName) {
    const url = window.URL.createObjectURL(blob);
    a.href = url;
    a.download = fileName;
    a.click();
  };
})();
```

另一种解决办法是 preserveDrawingBuffer: true，同时也需要告诉 three 不自动 clear canvas
Let's say you wanted to let the user paint with an animated object. You need to pass in preserveDrawingBuffer: true when you create the WebGLRenderer. This prevents the browser from clearing the canvas. You also need to tell three.js not to clear the canvas as well.

```js
const renderer = new THREE.WebGLRenderer({
  canvas,
  preserveDrawingBuffer: true,
  alpha: true,
});
renderer.autoClearColor = false;
```

### Getting Keyboard Input

```html
<canvas tabindex="0"></canvas>
```

### Making the Canvas Transparent

```js
const renderer = new THREE.WebGLRenderer({
  canvas,
  alpha: true,
});
```

Three.js defaults to the canvas using `premultipliedAlpha: true` but defaults to materials outputting `premultipliedAlpha: false`.

[图片 Premultiplied Alpha 到底是干嘛用的](https://segmentfault.com/a/1190000002990030)

https://developer.nvidia.com/content/alpha-blending-pre-or-not-pre

透明度混合公式

```
C_out = C_src * opacity + (1 - opacity) * C_dest
```

Premultiplied Alpha 后的像素格式变得不直观

混合的时候可以少一次乘法，这可以提高一些效率

没有 Premultiplied Alpha 的纹理无法进行 Texture Filtering（除非使用最近邻插值）。

```js
// 如果使用没有 Premultiplied Alpha 的颜色进行插值，那么结果就是：
((255,0,0,1)+(0,255,0,0.1))⋅0.5=(127,127,0,0.55)

// 如果绿色 Premultiplied Alpha，也就是 (0, 255 * 0.1, 0, 0.1)，和红色混合后：
((255,0,0,1)+(0,25,0,0.1))⋅0.5=(127,25,0,0.55)
```

所以 Premultiplied Alpha 最重要的意义是使得带透明度图片纹理可以正常的进行线性插值。这样旋转、缩放或者非整数的纹理坐标才能正常显示，否则就会像上面的例子一样，在透明像素边缘附近产生奇怪的颜色。

不是预乘透明度混合才正常么，为什么 three 是 false 才正常的呢？

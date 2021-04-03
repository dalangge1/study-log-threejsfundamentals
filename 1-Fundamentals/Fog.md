# Fog

Fog in a 3D engine is generally a way of fading to a specific color based on the distance from the camera.

```js
const scene = new THREE.Scene();
{
  const color = 0xffffff; // white
  const near = 10;
  const far = 100;
  scene.fog = new THREE.Fog(color, near, far);
}

const scene = new THREE.Scene();
{
  const color = 0xffffff;
  const density = 0.1;
  scene.fog = new THREE.FogExp2(color, density);
}
```

Fog 的颜色需要和背景颜色一致

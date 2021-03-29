# Lights

### AmbientLight

color = materialColor \* light.color \* light.intensity;

### HemisphereLight

the sky color if the surface of the object is pointing up and the ground color if the surface of the object is pointing down.

发现朝上使用天空颜色，朝下使用地的颜色

### DirectionalLight

平行光，法线和光照来源方向点乘再乘光样色强度

### PointLight

点光源，与平行光类似，只是光照来源方向通过世界坐标系下点和点光源相减所得，并且随距离衰减

### SpotLight

聚光灯，点光源+圆锥体限制，多了个朝向方向，根据点和点光源与朝向方向夹角判断是否能照射到，并且随距离衰减

### RectAreaLight

方形的荧光灯？只作用于 MeshStandardMaterial 和 MeshPhysicalMaterial

#### physicallyCorrectLights

One thing we didn't cover is that there is a setting on the WebGLRenderer called `physicallyCorrectLights`. It effects how light falls off as distance from light. It only affects `PointLight` and `SpotLight`. `RectAreaLight` does this automatically.

点光源开了`physicallyCorrectLights`还有三个属性可设置

```js
const color = 0xffffff;
const intensity = 1;
const light = new THREE.PointLight(color, intensity);
light.power = 800;
light.decay = 2;
light.distance = Infinity;
```

For lights though the basic idea is you don't set a distance for them to fade out, and you don't set intensity. Instead you set the power of the light in lumens and then three.js will use physics calculations like real lights. The units of three.js in this case are meters and a 60w light bulb would have around 800 lumens. There's also a decay property. It should be set to 2 for realistic decay.

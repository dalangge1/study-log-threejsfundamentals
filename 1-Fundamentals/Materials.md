# Materials

罗列 three 支持的颜色格式

```js
material.color.set(0x00FFFF);    // same as CSS's #RRGGBB style
material.color.set(cssString);   // any CSS color, eg 'purple', '#F32',
                                 // 'rgb(255, 127, 64)',
                                 // 'hsl(180, 50%, 25%)'
material.color.set(someColor)    // some other THREE.Color
material.color.setHSL(h, s, l)   // where h, s, and l are 0 to 1
material.color.setRGB(r, g, b)   // where r, g, and b are 0 to 1

const m1 = new THREE.MeshBasicMaterial({color: 0xFF0000});         // red
const m2 = new THREE.MeshBasicMaterial({color: 'red'});            // red
const m3 = new THREE.MeshBasicMaterial({color: '#F00'});           // red
const m4 = new THREE.MeshBasicMaterial({color: 'rgb(255,0,0)'});   // red
const m5 = new THREE.MeshBasicMaterial({color: 'hsl(0,100%,50%)'); // red
```

材质

0. MeshBasicMaterial（无光照影响）
1. MeshLambertMaterial（在顶点着色器计算光照）
2. MeshPhongMaterial（每个像素计算光照，支持高光）（shininess）
3. MeshToonMaterial（卡通着色，使用梯度映射着色）

把 MeshLambertMaterial 或者 MeshPhongMaterial 的 emissive 设置为颜色，
并且 color 设置为黑色，shininess 为 0 phong 的话，等同于 MeshBasicMaterial

有两种 PBR(Physically Based Rendering)

0. 简单 MeshStandardMaterial（roughness，metalness）
1. 复杂 MeshPhysicalMaterial（roughness，metalness，clearcoat）

渲染速度

MeshBasicMaterial ➡
MeshLambertMaterial ➡
MeshPhongMaterial ➡
MeshStandardMaterial ➡
MeshPhysicalMaterial

有另外 3 种材质有特殊使用

0. ShadowMaterial（从阴影获取数据？）
1. MeshDepthMaterial（渲染深度到纹理）
2. MeshNormalMaterial（显示几何的法线）

Material 基类属性

0. flatShading（法线插值）
1. side（THREE.FrontSide，THREE.BackSide，THREE.DoubleSide）
2. needsUpdate（flatShading 或者从无纹理到有纹理的切换）

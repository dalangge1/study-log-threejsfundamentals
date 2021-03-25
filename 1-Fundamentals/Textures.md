# Textures

### Hello Texture & Loading Textures

使用 TextureLoader 加载图片作为纹理

BoxGeometry 支持 6 张纹理
ConeGeometry 支持 2 张纹理
CylinderGeometry 支持 3 张纹理

```js
// prettier-ignore
const materials = [
  new THREE.MeshBasicMaterial({map: loader.load('https://threejsfundamentals.org/threejs/resources/images/flower-1.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('https://threejsfundamentals.org/threejs/resources/images/flower-2.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('https://threejsfundamentals.org/threejs/resources/images/flower-3.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('https://threejsfundamentals.org/threejs/resources/images/flower-4.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('https://threejsfundamentals.org/threejs/resources/images/flower-5.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('https://threejsfundamentals.org/threejs/resources/images/flower-6.jpg')}),
];
const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
const cube = new THREE.Mesh(geometry, materials);
```

### Memory Usage

textures take width _ height _ 4 \* 1.33 bytes of memory.(乘以 1.33 是 mipmap 的占用)

### JPG vs PNG

PNG 支持无损压缩，同时支持透明通道

### Filtering and Mips

mipmap 的 6 种采样方式，WebGL fundamentals 讲过

### Repeating, offseting, rotating, wrapping a texture

纹理溢出(0, 1)的表现方式：ClampToEdgeWrapping | RepeatWrapping | MirroredRepeatWrapping

```js
const xOffset = 0.5; // offset by half the texture
const yOffset = 0.25; // offset by 1/4 the texture
someTexture.offset.set(xOffset, yOffset);

someTexture.center.set(0.5, 0.5);
someTexture.rotation = THREE.MathUtils.degToRad(45); // 支持纹理的偏移和旋转
```

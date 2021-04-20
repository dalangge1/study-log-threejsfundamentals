# Optimize Lots of Objects

`merging geometry`

Drawing 2 things has more overhead than drawing 1 even if the results are the same so one way to optimize is to merge meshes.

世界人口可视化

```js
/**
 * 绘制三维的经纬度，通过3个helper实现
 * lonHelper - latHelper - positionHelper
 * 通过修改helper变换，计算出点的变化矩阵matrixWorld，然后应用到点
 */
function addBoxes(file) {
  const { min, max, data } = file;
  const range = max - min;

  // make one box geometry
  const boxWidth = 1;
  const boxHeight = 1;
  const boxDepth = 1;
  const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
  // make it so it scales away from the positive Z axis
  geometry.applyMatrix4(new THREE.Matrix4().makeTranslation(0, 0, 0.5));

  // these helpers will make it easy to position the boxes
  // We can rotate the lon helper on its Y axis to the longitude
  const lonHelper = new THREE.Object3D();
  scene.add(lonHelper);
  // We rotate the latHelper on its X axis to the latitude
  const latHelper = new THREE.Object3D();
  lonHelper.add(latHelper);
  // The position helper moves the object to the edge of the sphere
  const positionHelper = new THREE.Object3D();
  positionHelper.position.z = 1;
  latHelper.add(positionHelper);

  const lonFudge = Math.PI * 0.5;
  const latFudge = Math.PI * -0.135;
  data.forEach((row, latNdx) => {
    row.forEach((value, lonNdx) => {
      if (value === undefined) {
        return;
      }
      const amount = (value - min) / range;
      const material = new THREE.MeshBasicMaterial();
      const hue = THREE.MathUtils.lerp(0.7, 0.3, amount);
      const saturation = 1;
      const lightness = THREE.MathUtils.lerp(0.1, 1.0, amount);
      material.color.setHSL(hue, saturation, lightness);
      const mesh = new THREE.Mesh(geometry, material);
      scene.add(mesh);

      // adjust the helpers to point to the latitude and longitude
      lonHelper.rotation.y = THREE.MathUtils.degToRad(lonNdx + file.xllcorner) + lonFudge;
      latHelper.rotation.x = THREE.MathUtils.degToRad(latNdx + file.yllcorner) + latFudge;

      // use the world matrix of the position helper to
      // position this mesh.
      positionHelper.updateWorldMatrix(true, false);
      mesh.applyMatrix4(positionHelper.matrixWorld);

      mesh.scale.set(0.005, 0.005, THREE.MathUtils.lerp(0.01, 0.5, amount));
    });
  });
}
```

使用 BufferGeometryUtils 来合并 geometry

```js
import { BufferGeometryUtils } from './resources/threejs/r125/examples/jsm/utils/BufferGeometryUtils.js';
```

颜色使用 vertex colors

### 问题

matrixWorld 和 worldMatrix 还是比较容易混乱

matrixWorld：The global transform of the object. If the Object3D has no parent, then it's identical to the local transform
worldMatrix：没有这个属性

```js
.updateMatrixWorld(): null
  // Updates the global transform of the object and its descendants.
  // 更新自身和子节点的`global transform`在世界坐标系下的变换矩阵
.updateWorldMatrix(updateParents: Boolean, updateChildren: Boolean): null
  // updateParents - recursively updates global transform of ancestors.
  // updateChildren - recursively updates global transform of descendants.
  // 更新父/子的`global transform`在世界坐标系下的变换矩阵
```

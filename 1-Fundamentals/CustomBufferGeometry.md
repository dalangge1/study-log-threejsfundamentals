# CustomBufferGeometry

0. position
1. normal
2. color
3. uv

```js
const geometry = new THREE.BufferGeometry();
const positionNumComponents = 3;
const normalNumComponents = 3;
const uvNumComponents = 2;
geometry.setAttribute(
  'position',
  new THREE.BufferAttribute(new Float32Array(positions), positionNumComponents),
);
geometry.setAttribute(
  'normal',
  new THREE.BufferAttribute(new Float32Array(normals), normalNumComponents),
);
geometry.setAttribute('uv', new THREE.BufferAttribute(new Float32Array(uvs), uvNumComponents));
```

BufferGeometry has a computeVertexNormals method for computing normals if you are not supplying them.

```js
const positionAttribute = new THREE.BufferAttribute(positions, positionNumComponents);
positionAttribute.setUsage(THREE.DynamicDrawUsage);
```

I've highlighted a few differences. We save a reference to the position attribute. We also mark it as dynamic. This is a hint to THREE.js that we're going to be changing the contents of the attribute often.

最后的 demo 很酷炫

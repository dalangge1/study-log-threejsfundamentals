# Shadows

Three.js by default uses shadow maps. The way a shadow map works is, for every light that casts shadows all objects marked to cast shadows are rendered from the point of view of the light. READ THAT AGAIN! and let it sink in.

In other words, if you have 20 objects, and 5 lights, and all 20 objects are casting shadows and all 5 lights are casting shadows then your entire scene will be drawn 6 times. All 20 objects will be drawn for light #1, then all 20 objects will be drawn for light #2, then #3, etc and finally the actual scene will be drawn using data from the first 5 renders.

For these reasons it's common to find other solutions than to have a bunch of lights all generating shadows. One common solution is to have multiple lights but only one directional light generating shadows.

Yet another solution is to use lightmaps and or ambient occlusion maps to pre-compute the effects of lighting offline. This results in static lighting or static lighting hints but at least it's fast. We'll cover both of those in another article.

Another solution is to use fake shadows. Make a plane, put a grayscale texture in the plane that approximates a shadow, draw it above the ground below your object.

So, moving on to shadow maps, there are 3 lights which can cast shadows. The DirectionalLight, the PointLight, and the SpotLight.

You can set the resolution of the shadow map's texture by setting light.shadow.mapSize.width and light.shadow.mapSize.height. They default to 512x512.

```js
const renderer = new THREE.WebGLRenderer({ canvas });
renderer.shadowMap.enabled = true;

const light = new THREE.DirectionalLight(color, intensity);
light.castShadow = true;

const mesh = new THREE.Mesh(planeGeo, planeMat);
mesh.receiveShadow = true;

const mesh = new THREE.Mesh(cubeGeo, cubeMat);
mesh.castShadow = true;
mesh.receiveShadow = true;
```

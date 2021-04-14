# Debugging GLSL

把法线坐标可视化出来

```glsl
gl_FragColor = vec4(vNormal * 0.5 + 0.5, 1);
```

把纹理坐标可视化

```glsl
gl_FragColor = vec4(fract(vUv), 0, 1);
```

所以只需要知道想要了解的值的取值范围，然后归一化之后可视化出来
You can do similar things for all values in your fragment shader. Figure out what their range is likely to be, add some code to set gl_FragColor with that range scaled to 0.0 to 1.0

注意数据是否 NaN，比如rotation.xyz某个值为NaN，会导致白屏


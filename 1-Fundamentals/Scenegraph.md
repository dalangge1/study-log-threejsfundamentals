# Scenegraph

实现一个太阳系

```
Scene
  solarSystem
    sunMesh
    earthOrbit
      earthMesh
      moonOrbit
        moonMesh
```

camera 往下看，需要把 up 设置为(0,0,1)用于确定 x 轴方向（z 轴不能与 up 方向同一方向）

对节点变化是变化其坐标系，包含子节点

## 问题

骨骼和场景节点是同一个东西么？


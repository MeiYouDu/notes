# 简介

`geometry`属性决定了`primitive`几何结构，比如组成`primitive`的点、线、三角。

`appearance`决定了`primitive`的着色，它包含了全部的`GLSL`顶点着色器和片段着色器以及渲染状态。

# 使用这些 `api`的优点。

- performance，当绘制大量静态`primitive`(例如美国每个邮政编码的多边形)时，直接使用`geometries`可以让我们将它们组合成单个几何图形，以减少CPU开销并更好地利用GPU。组合`primitive`是在web worker上完成的，以保持UI的响应性。

- flexibility，`primitive`结合了几何和外观。通过解耦，我们可以独立地修改它们。我们可以添加与许多不同外观兼容的新几何形状，反之亦然。

- low-level access，`appearance`提供了接近原生的渲染访问，而不必担心直接使用Renderer的所有细节。

  `appearance`的优点:

  -编写完整的GLSL顶点和片段着色器。

  -使用自定义渲染状态。

# **缺点**

- 直接使用几何图形和外观需要更多的代码和对图形更深入的理解。实体处于适合于地图应用的抽象级别;几何形状和外观的抽象程度更接近于传统的3D引擎。
- 组合几何对静态数据有效，但对动态数据不一定有效。

# Appearance

![Geometry and appearances highleveldesign](https://raw.githubusercontent.com/yqm1995/pic_bed/master/images/tutorials-geometry-and-appearances-highleveldesign-20240912143853463.jpeg)

如图所示，一个 `primitive`可以拥有多个 `geometryInstance`，每个 `geometryInstance`只能包含一个 `geometry`；一个 `primitive`可以也只能拥有一个 `appearance`，根据外观的类型，外观将有一个材料来定义阴影的体积。

# cesium 中的 appearance 类型

| Appearance                   | Description                                                  |
| :--------------------------- | :----------------------------------------------------------- |
| `MaterialAppearance`         | An appearance that works with all geometry types and supports materials to describe shading.<br />一种适用于所有几何类型的外观，并支持描述阴影的材料。 |
| `EllipsoidSurfaceAppearance` | A version of `MaterialAppearance `that assumes geometry is parallel to the surface of the globe, like a polygon, and uses this assumption to save memory by procedurally computing many vertex attributes.<br />“MaterialAppearance”的一个版本，它假设几何形状与地球表面平行，就像一个多边形，并使用这个假设通过程序计算许多顶点属性来节省内存。 |
| `PerInstanceColorAppearance` | Uses per-instance color to shade each instance.<br />使用每个实例的颜色来着色每个实例。 |
| `PolylineMaterialAppearance` | Supports materials to shade a Polyline.<br />支持材质来着色折线 |
| `PolylineColorAppearance`    | Uses either per-vertex or per-segment coloring to shade a Polyline.<br />使用每个顶点或者片段颜色来着色折线 |

`appearance`定义了绘制`primitive`时在GPU上执行的完整GLSL顶点和片段着色器。`appearance`还定义了完整的渲染状态，它控制了绘制`primitive`时GPU的状态。我们可以直接定义渲染状态，或者使用更高级的属性，比如closed和半透明，这些属性会被转换成渲染状态。

Most appearances also have [flat](https://cesium.com/learn/cesiumjs/ref-doc/MaterialAppearance.html#flat) and [faceForward](https://cesium.com/learn/cesiumjs/ref-doc/MaterialAppearance.html#faceForward) properties, which indirectly control the GLSL shaders.

- flat - Flat shading. Do not take lighting into account.
- faceForward - When lighting, flip the normal so it is always facing the viewer. The avoids black areas on back-faces, e.g., the inside of a wall.

![img](https://raw.githubusercontent.com/yqm1995/pic_bed/master/images/5fc89576-3032-4f95-8187-fce2a2b6cd17_Geometries-flat.jpeg)

# geometry 和 appearance 的兼容性

并不是任何 `appearance`都能搭配任何 `geometry`一起使用，比如 `EllipsoidSurfaceAppearance` 不能跟 `WallGeometry` 一起使用，因为墙不可能在是一个球状的表面。

`appearance`为了兼容一个 `geometry`，必须有匹配的顶点格式，换句话说 `geometry`必须拥有 `appearance`期望输入的数据。可以通过 VertexFormat 来确定 `geometry` 的几何格式。

![Geometry and appearances compatible](https://raw.githubusercontent.com/yqm1995/pic_bed/master/images/tutorials-geometry-and-appearances-compatible-20240912155210552.jpeg)

![Geometry and appearances notCompatible](https://raw.githubusercontent.com/yqm1995/pic_bed/master/images/tutorials-geometry-and-appearances-notCompatible-20240912155224625.jpeg)

`geometry`的`vertexFormat`决定了它是否可以与另一个`geometry`组合。两个`geometry`不必是相同的类型，但它们需要匹配的`vertexFormat`。

为方便起见，`appearance`可以有一个`vertexFormat`属性或一个VERTEX_FORMAT静态常量，可以作为一个选项传递给几何图形。

```javascript
const geometry = new Cesium.RectangleGeometry({
  vertexFormat: Cesium.EllipsoidSurfaceAppearance.VERTEX_FORMAT,
  // ...
});

const geometry2 = new Cesium.RectangleGeometry({
  vertexFormat: Cesium.PerInstanceColorAppearance.VERTEX_FORMAT,
  // ...
});

const appearance = new Cesium.MaterialAppearance(/* ... */);
const geometry3 = new Cesium.RectangleGeometry({
  vertexFormat: appearance.vertexFormat,
  // ...
});
```

# 更新效率实验

## 更新 primitive 的 modelMatrix

当时用多个 `geometry`的情况下，为了达到更新 `primitive` 的 `modelMatrix` 就可以改变 `geometry` 的位置（修改 primitive 的 modelMatrix 会改变所有 geometryinstance 的位置）。所以要创建多个 `primitive`，每个 `primitive` 对应一个 `geometry`。

1000 个棱锥和棱锥外框线，通过更新 `modelMatrix` 的方式。得到下面这个效果

![image-20240913095826489](https://raw.githubusercontent.com/yqm1995/pic_bed/master/images/image-20240913095826489.png)

 缺点：

- 无法更新图形的几何结构，只能更新位置和姿态。

这种方案在更新时可以充分利用gpu 的能力。

## 更新 geometryInstance

创建一个 `primitive` ，移除后使用同一个 `geometry` 根据新的位置创建一批新的 `geometryInstance` 。

### 同步创建模式

使用两个 primitive，一个绘制 frustum，一个绘制 frustumOutline，将所有 geometryInstance 组合到一起，效果如下，这种创建方案也可以充分利用 gpu 性能。

![image-20240913114350770](https://raw.githubusercontent.com/yqm1995/pic_bed/master/images/image-20240913114350770.png)

### 通过 cpu 数量计算分组

跟上面的方式差距不大

### 异步创建 primitive

每一帧都重新创建 `primitive`，这个方案表面上看帧率最高，但是会有图形闪烁的问题，通过双缓冲或者三缓冲优化可以缓解这个问题但仍然有轻微的闪烁问题。

## 总结

`primitive`图形更新问题可以拆分为两个问题（更新颜色啥的不做讨论）

- 一个是单纯的更新图形的位置和姿态
- 更新图形结构本身

目前的最佳实践是，只更新位置和姿态就是用 `primitive` 的 `modelMatrix` ，如果要更新集合体本身，就使用同步的方式创建新的 geometryInstance，还需要结合双缓冲来减轻闪烁问题。
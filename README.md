<!--
 * @Author: yaolinhong yaolinhong1024@gmail.com
 * @Date: 2023-11-20 21:01:25
 * @LastEditors: yaolinhong yaolinhong1024@gmail.com
 * @LastEditTime: 2023-11-20 22:25:32
 * @FilePath: \revit-three.js\README.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
## 目的

mvp
revit模型 -> glb -> 渲染模型、点击获取构件信息

## 主要技术栈
- Vue3 + Ts + Vite
- three封装库 trois.js Read more on https://troisjs.github.io/guide/

## 导出revit模型
[revit插件](https://prototechsolutions.com/3d-products/revit/gltf-exporter)

<img src="https://github.com/yaolinhong/revit-three.js/blob/main/src/assets/gltf_exporter.png?raw=true" />


## 模型压缩 添加模型信息
* 模型压缩
<img src="https://github.com/yaolinhong/revit-three.js/blob/main/src/assets/blender_compression.png?raw=true" />

* 添加模型信息
<img src="https://github.com/yaolinhong/revit-three.js/blob/main/src/assets/change_mesh_name.png?raw=true" />

## 模型导入、交互 见App.vue

## 迭代日志

## 2023-11-20
### Add
feat: 模型压缩调研 gltf-transform

```
gltf-transform optimize ./revit.glb ./revit-compressed.glb --texture-compress webp
<!-- 一键压缩效果 info: revit.glb (15.15 MB) → revit-compressed.glb (390.81 KB) -->
<!-- 报错 console.error('Missing parent Mesh') -->

gltf-transform draco  ./revit.glb ./revit-compressed.glb
<!-- draco算法优化 -->
<!-- info: revit.glb (15.15 MB) → revit-compressed.glb (1.46 MB) 正常加载 -->
```

感兴趣的朋友可以了解一下，压缩的技术

* [webgl_loader_draco](https://github.com/mrdoob/three.js/blob/master/examples/webgl_loader_draco.html)


* [gltf-transform](https://www.npmjs.com/package/@gltf-transform/cli)
* [gltf-pipeline](https://github.com/CesiumGS/gltf-pipeline)

#### 压缩方式基本原理
1. 减少模型的多边形数量：多边形数量越多，模型的复杂度和文件大小就会增加。你可以使用3D建模软件，如Blender或Maya，来减少模型中的多边形数量。这可以通过简化几何形状、合并顶点或使用LOD（Level of Detail）技术来实现。

2. 压缩纹理：纹理是模型中常用的图像贴图，可以给模型增加细节和颜色。你可以使用图像压缩工具，如JPEG或PNG压缩算法，来减小纹理文件的大小。同时，还可以考虑使用纹理压缩格式，如Draco 压缩算法、DDS或ASTC，以减少文件大小。

3. 移除不必要的数据：在GLTF/GLB文件中，可能包含一些不必要的数据，如动画、骨骼、材质等。如果你的应用程序不需要这些数据，可以考虑在导出模型时将其移除，以减小文件大小。

4. 优化模型结构：检查模型的结构，确保没有重复的顶点或面片。优化模型结构可以减小文件大小，并提高渲染性能。

5. 使用压缩工具：你可以使用GLTF/GLB的压缩工具来进一步减小文件大小。这些工具可以对模型进行压缩和优化，以提高加载性能和减少网络传输时间。

6. 图片压缩webp格式

7. 使用drc加载

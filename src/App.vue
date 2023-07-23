<template>
  <Renderer :orbit-ctrl="{ enableDamping: true, dampingFactor: 0.05 }" style="position: relative;" ref="renderer"
    :pointer="{ intersectRecursive: true }" @click="onClickModel" resize="window">
    <div class="button-wrapper">

      <br>
      <button @click="setHightLight">全部构件变色</button>
      <br>
      <br>
      点击构件变色、展示构件name
      <br>
      <br>
      <div>
        ReachMe: yaolinhong1024@gmail.com
      </div>
      <br>
      <br>
      <div>
        github: <a href="https://github.com/yaolinhong" target="_blank">https://github.com/yaolinhong</a>
      </div>
    </div>
    <Camera :position="{ x: 100, y: 50, z: 200 }" />
    <Scene>
      <Text text="Hello Revit" font-src="/src/assets/models/font.json" align="center" :size="20" :height="5"
        :position="{ x: 0, y: 50, z: 0 }" :cast-shadow="true">
        <PhongMaterial />
      </Text>
      <!-- <Raycaster intersect-recursive @click="onClickRaycaster" /> -->
      <AmbientLight color="#aaaaaa" />
      <SpotLight color="#aaaaaa" :intensity="0.5" :position="{ x: 0, y: 1, z: 2 }" :angle="Math.PI / 3" :penumbra="0.5"
        cast-shadow :shadow-map-size="{ width: 1024, height: 1024 }" />
      <SpotLight color="#ff0000" :intensity="0.5" :position="{ x: 0, y: -1, z: 2 }" :angle="Math.PI / 3" :penumbra="0.5"
        cast-shadow :shadow-map-size="{ width: 1024, height: 1024 }" />

      <PointLight ref="light" distance="8000" decay="200" color="#fff"></PointLight>
      <!-- <AmbientLight /> -->
      <GLTF @click="onClickRaycaster" @load="onReady" ref="gltf" dracoPath="/src/assets/models/revit.glb"
        src="/src/assets/models/revit.glb">
        <LambertMaterial />
      </GLTF>
    </Scene>
    <!-- <EffectComposer>
      <RenderPass />
      <UnrealBloomPass :strength="0.1" />
      <FXAAPass />
    </EffectComposer> -->
  </Renderer>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { Box, Camera, Text, LambertMaterial, MeshPublicInterface, PointLight, Renderer, RendererPublicInterface, Scene, EffectComposer, FXAAPass, RenderPass, UnrealBloomPass, AmbientLight } from './export'
import GLTF from './models/GLTF';
import { Color } from 'three';
import { useDebounceFn } from '@vueuse/core'

// Tweakpane 控制图形颜色
export default defineComponent({
  components: {
    Box, Camera, LambertMaterial, PointLight, Text, Renderer, Scene, GLTF, EffectComposer, FXAAPass, RenderPass, UnrealBloomPass, AmbientLight
  },
  data() {
    return {
      model: null,
      light: null,
      renderer: null
    }
  },
  setup() {
    const onClickRaycaster = useDebounceFn(function (...e) {
      const intersect = e[0].intersect
      const target = e[0].intersect.object.material
      console.log('🚀 - onClickRaycaster - target:', intersect.object, intersect.object.name)
      if (target) target.color = new Color(0x007cff);
      window.alert('选中了' + intersect.object.name)
    }, 200)

    return { onClickRaycaster }
  },
  mounted() {
    const renderer = this.$refs.renderer as RendererPublicInterface
    const gltf = this.$refs.gltf;
    this.light = this.$refs.light.light;
    this.renderer = renderer;

    renderer.onMounted(() => {
      console.log('🚀 - mounted - gltf:', gltf)

    })
    renderer.onMounted(() => {
      console.log('🚀 - mounted - gltf:', gltf)

    })

    renderer.onBeforeRender(this.animate)
    // const mesh = (this.$refs.gltf as MeshPublicInterface).mesh
    // if (renderer && mesh) {
    //   renderer.onBeforeRender(() => {
    //     mesh.rotation.x += 0.01
    //   })
    // }
  },
  methods: {
    onClickModel(...e) {
      // console.log('🚀 - onClickModel - e:', e)
      // const { pointer } = this.renderer.three;
      // console.log('🚀 - onClickModel - pointer:', pointer)
      // const target = pointer.intersectObjects[0]
      // console.log('🚀 - onClickModel - target:', target)
      // if (target) target.material.color = new Color(0x007cff);

    },
    animate() {
      const { pointer } = this.renderer.three;
      // if (this.target) this.target.copy(pointer.positionV3)
      if (this.light) {
        // console.log('🚀 - animate - pointer:', pointer.positionV3)

        this.light?.position.copy(pointer.positionV3);
      }
    },
    setHightLight() {
      // console.log('🚀 - this.model.scene:555', this.model.scene);
      // const target = this.model.scene.children.find(item => item.material)
      // console.log('🚀 - setHightLight - target:', target)
      // target.color = new Color(0x007cff);
      // this.model.scene.children[0].material.color = new Color(0x007cff);
      (this.model.scene)?.traverse((object) => {
        // Check if the object is the desired component
        // if (object.name === 'componentName') {
        // Apply hover and highlight effects to the component
        if (object.material) {
          console.log('🚀 - this.model.scene?.traverse - object.material:', object.material)
          console.log('🚀 - object.uuid:', object.uuid)
          object.material.color = new Color(0x007cff);
        } // Change color to red
        // Add any other desired effects
        // }
      });


      //       const canvas = renderer.domElement;

      // canvas.addEventListener('mousemove', (event) => {
      //   // Calculate the mouse position relative to the canvas
      //   const rect = canvas.getBoundingClientRect();
      //   const mouseX = event.clientX - rect.left;
      //   const mouseY = event.clientY - rect.top;

      //   // Use the mouse position to determine if it is over the component
      //   if (mouseIsOverComponent(mouseX, mouseY)) {
      //     // Apply the hover effect
      //     applyHoverEffect(component);
      //   } else {
      //     // Remove the hover effect
      //     removeHoverEffect(component);
      //   }
      // });

    },
    onReady(model) {
      this.model = model
      console.log('🚀 - onReady - model:', model)
      model.scene?.traverse(() => {
        // console.log('🚀 - onReady - o:', child)
        // if (child.isMesh) {
        //   console.count('child')
        //   console.log('🚀 - model.scene?.traverse - child:', child)
        //   meshes.push(child);
        //   // 石头底座样式
        //   if (/石头/.test(child.name)) {
        //     child.material.transparent = true
        //   }
        //   // 高亮气旋
        //   if (/气旋/.test(child.name)) {
        //     child.material.transparent = true;
        //     child.emissive = new THREE.Color(0x0000ff);
        //     child.emissiveIntensity = 2;
        //   }
        //   // link 右手
        //   if (child.name === 'Mesh_5') {
        //     child.material.color = new THREE.Color(0x007cff);
        //     child.material.emissive = child.material.color;
        //   }
        // }

      })
      //   if (o.isMesh) {
      //     // handle both multiple and single materials
      //     const asArray = Array.isArray(o.material) ? o.material : [o.material]
      //     // 0 works for matte materials - change as needed
      //     asArray.forEach(mat => mat.metalness = 0)
      //   }
      // })
    },
    gltfOver({ over }: any) {
      console.log('🚀 - gltfOver - over:', over)
      // this.gltfColor = over ? '#ff0000' : '#ffffff';
    },
    gltfClick(...e: any[]) {
      alert('Click');
      console.log(e);
    }
  }
})
</script>

<style>
body,
html {
  margin: 0;
}

canvas {
  display: block;
}

.button-wrapper {
  padding: 10px;
  color: #fff;
  position: absolute;
  border: 1px solid red;
  z-index: 99999;
  top: 0;
  left: 0;

  button {
    cursor: pointer;
  }

}</style>

<template>
  <Renderer ref="renderer" resize="window" antialias :orbit-ctrl="{ autoRotate: true, enableDamping: true, dampingFactor: 0.05 }" pointer shadow>
    <Camera :position="{ x: 0, y: 0, z: 10 }" />
    <Scene background="#000000" >
      <PointLight ref="light" cast-shadow :shadow-map-size="{ width: 1024, height: 1024 }" :intensity="1" :position="{ x: 0, y: 0, z: 0 } ">
        <Sphere :radius="0.1" />
      </PointLight>        
      <RectAreaLight color="#ff00ff" :intensity="10" :position="{ x: 0, y: 3, z: 1 }" v-bind="rectLightsProps" />
      <Plane :width="10" :height="10" :rotation="{ x: -Math.PI / 2 }" :position="{ y: -1.05 }" receive-shadow>
        <StandardMaterial :props="{ displacementScale: 0.2, roughness: 1, metalness: 0 }" >
          <Texture :props="texturesProps" src="/assets/textures/PolishedConcrete01_4K_BaseColor.png" />
          <Texture :props="texturesProps" src="/assets/textures/PolishedConcrete01_4K_Height.png" name="displacementMap" />
          <Texture :props="texturesProps" src="/assets/textures/PolishedConcrete01_4K_Normal.png" name="normalMap" />
          <Texture :props="texturesProps" src="/assets/textures/PolishedConcrete01_4K_Roughness.png" name="roughnessMap" />
          <Texture :props="texturesProps" src="/assets/textures/PolishedConcrete01_4K_Height.png" name="aoMap" />
        </StandardMaterial>
      </Plane>
      <GltfModel :src="'/assets/models/PyramidSetup.gltf'" :position="{ x: 0, y: 0, z: 0 }" :rotation="{ x: 0, y: 0, z: 0 }" :scale="{ x: 1, y: 1, z: 1 }"  />
    </Scene>
    <EffectComposer>
      <RenderPass />
      <UnrealBloomPass :strength="0.5" />
      <FXAAPass />
    </EffectComposer>
  </Renderer>
</template>

<script>

import { RepeatWrapping } from 'three';
import {
  AmbientLight,
  Camera,
  EffectComposer,
  FXAAPass,
  Group,
  Renderer,
  Plane,
  PointLight,
  RectAreaLight,
  RenderPass,
  Scene,
  Sphere,
  StandardMaterial,
  Texture,
  UnrealBloomPass,
  GltfModel
} from 'troisjs';
export default {
  components: {
    AmbientLight,
    Camera,
    EffectComposer,
    FXAAPass,
    Group,
    Renderer,
    Plane,
    PointLight,
    RectAreaLight,
    RenderPass,
    Scene,
    Sphere,
    StandardMaterial,
    Texture,
    UnrealBloomPass,
    GltfModel
  },
  data() {
    return {
      texturesProps: {
        repeat: { x: 2, y: 2 },
        wrapS: RepeatWrapping,
        wrapT: RepeatWrapping,
      },
      rectLightsProps: {
        // rotation: { x: -Math.PI / 2 },
        lookAt: { x: 0, y: 0, z: 1 },
        //intensity: 5,
        width: 1,
        height: 1,
        helper: true,
      },
    };
  },
  mounted() {
    const renderer = this.$refs.renderer;
    const light = this.$refs.light.light;
    const pointerV3 = renderer.three.pointer.positionV3;
    renderer.onBeforeRender(() => {
      light.position.copy(pointerV3);
    });
  },
};
</script>

<style>
body {
  margin: 0;
}
canvas {
  display: block;
}
</style>
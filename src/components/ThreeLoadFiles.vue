<template>
<h1>this is showing how to  load 3d file & responsive</h1>
  <!--
   container for displaying 3D content
 -->
  <div ref="container" class="container"></div>
</template>

<script>

import {ref, onMounted, onUnmounted} from 'vue';
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';


export default {
  name: "ThreeLoadFiles",

  setup() {
    // 创建容器
    const container = ref(null);
    // 创建场景
    const scene = new THREE.Scene();
    // 设置场景背景颜色
    scene.background = new THREE.Color(0xff00ff);

    // 创建相机
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer();
    // 设置渲染器大小
    renderer.setSize(window.innerWidth, window.innerHeight);

    let mesh;
    // 创建加载器
    const loader = new GLTFLoader();

    onMounted(() => {
      container.value.appendChild(renderer.domElement);
      // 创建光源
      const light = new THREE.PointLight(0xffffff, 1, 100);
      light.position.set(0, 0, 5);
      // 将光源添加到场景
      scene.add(light);


      // 加载器加载 3D 资源模型
      loader.load(
          // path to the .glb file, NOTE: it goes to the public dir
          'model/hero.glb',
          // called when the resource is loaded
          function (gltf) {
            mesh = gltf.scene;
            scene.add(mesh);
            camera.position.set(0, 0.1, 1.1)
            animate();
          },
          // called while loading is progressing
          function (xhr) {
            console.log((xhr.loaded / xhr.total * 100) + '% loaded');
          },
          // called when loading has errors
          function (error) {
            console.log('An error happened' +error);
          }
      );
      // 处理响应式
      window.addEventListener('resize', handleResize);
    });

    onUnmounted(() => {
      window.removeEventListener('resize', handleResize);
    });

    const animate = () => {
      requestAnimationFrame(animate);
      if (mesh) {
        // mesh.rotation.x += 0.01;
        mesh.rotation.y -= 0.01;

        // mesh.rotation.x = 0.01;
        // mesh.rotation.y = 0.01;
      }
      renderer.render(scene, camera);
    };

    const handleResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    };

    return {
      container
    }
  }
}
</script>

<style scoped>
.container{
  overflow: hidden;
}

</style>

<template>
  <h1>this is an example of how to combine Three.js with Vue3.js</h1>
  <h3>composition api</h3>

  <!--
    container for displaying 3D content
  -->
  <div ref="container" class="container"></div>
</template>

<script>
import {ref, onMounted, onUnmounted} from 'vue';
import * as THREE from 'three';

export default {
  name: "VueThree",
  setup() {
    const container = ref(null);
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    const geometry = new THREE.BoxGeometry();
    const material = new THREE.MeshBasicMaterial({ color: 0xffffff,  wireframe: true  });
    const mesh = new THREE.Mesh(geometry, material);

    onMounted(() => {
      container.value.appendChild(renderer.domElement);
      scene.add(mesh);
      camera.position.z = 5;
      animate();
      window.addEventListener('resize', handleResize);
    });

    onUnmounted(() => {
      window.removeEventListener('resize', handleResize);
    });

    const animate = () => {
      requestAnimationFrame(animate);
      mesh.rotation.x += 0.01;
      mesh.rotation.y += 0.01;
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
};
</script>

<style scoped>
.container {
  overflow: hidden;
}

</style>

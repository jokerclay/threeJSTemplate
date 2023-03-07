<template>
  <hr>
  <h1>this is a  native three example</h1>
  <hr>

  <div class="wrapper">

    <!--
     展示 3D 的容器
    -->
    <div ref="container" class="container"></div>

  </div>

</template>

<script>

// 将 three.js 导入这个组件
import * as THREE from 'three';

export default {
  name: "NativeThree",
  // 每次这个组件被挂载，就执行这个函数里面的语句
  mounted() {
    // 创建 renderer
    const renderer = new THREE.WebGLRenderer();
    // 将这个 renderer append 到  DOM 上
    this.$refs.container.appendChild(renderer.domElement);

    // 设置 renderer 的 大小
    const renderer_width = window.innerWidth;
    const renderer_height = window.innerHeight;
    renderer.setSize(renderer_width,renderer_height)


    // 创建 camera
    // 这个相机采用 PerspectiveCamera (透视摄像机)
    // fov 是视野范围(field of view)
    // aspect 指画布的宽高比
    // near 代表近平面
    // far 代表远平面
    // 近平面 和 远平面图示  https://threejs.org/manual/resources/frustum-3d.svg
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

    // 控制 相机的位置
    camera.position.z = 5;

    // 创建场景
    const scene = new THREE.Scene();

    // 创建立方体
    const geometry = new THREE.BoxGeometry();
    const material = new THREE.MeshBasicMaterial({ color: 0x00ffff });
    const cube = new THREE.Mesh(geometry, material);

    // 将立方体加入方块
    scene.add(cube);

    // 渲染立方体和方块
    renderer.render(scene, camera);

    // Animate the cube
    const animate = () => {
      requestAnimationFrame(animate);
      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;
      renderer.render(scene, camera);
    };

    animate();


    // 监听 window 的 resize 事件， 实时更新 renderer 的大小，使其具有响应式
    window.addEventListener('resize', () => {
      console.log("renderer width 是 "+ window.innerWidth)
      console.log("renderer height 是 "+ window.innerHeight)
      renderer.setSize(window.innerWidth, window.innerHeight);
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
    });



  },
}
</script>

<style scoped>
.wrapper {
  width: 100%;
  height: 100%;
}

.container{
  width: 100%;
  height: 100%;
  overflow: hidden;
}

</style>

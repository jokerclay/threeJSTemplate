<template>
  <h1>this is the orbit control example</h1>
  <!--
   container for displaying 3D content
 -->
  <div ref="container" class="container"></div>
</template>

<script>
import {ref, onMounted, onUnmounted} from "vue";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";


export default {
  name: "ThreeControl",
  setup() {
    // 创建容器
    const container = ref(null);
    // 声明所需的变量
    let scene, camera, renderer, model, controls;

    //  onMounted 的时候
    //      初始化 :
    //            场景
    //            透视相机
    //            渲染器
    //            append 到 dom 树
    //            设置控制属性
    //            处理响应式
    //      加载模型 :
    //            加载器加载模型
    //            渲染模型
    //
    onMounted(() => {
      init();
      loadModel();
    });

    const init = () => {
      // 创建场景
      scene = new THREE.Scene();
      // 设置场景背景颜色
      scene.background = new THREE.Color(0xff00ff);

      // 创建相机
      camera = new THREE.PerspectiveCamera(
          75,
          window.innerWidth / window.innerHeight,
          0.1,
          1000
      );
      // 默认的相机位置
      camera.position.set(0, 0.1, 1.1)

      // 创建渲染器
      renderer = new THREE.WebGLRenderer();
      // 设置渲染器大小
      renderer.setSize(window.innerWidth, window.innerHeight);

      // append canvas
      container.value.appendChild(renderer.domElement);

      // Create OrbitControls and add them to the camera
      controls = new OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;
      // controls.dampingFactor = 0.1;
      // controls.rotateSpeed = 1;
      // controls.zoomSpeed = 1.0;
      // controls.panSpeed = 2.0;

      // Create a new light and add it to the scene
      // // 创建光源
      // light = new THREE.PointLight(0xffffff, 0.9, 100);
      // light.position.set(2, 2, 5);
      // // 将光源添加到场景
      // scene.add(light);

      /*
      *  用两个 前后方向光  解决 只能看见一面的问题
      * */
      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
      directionalLight.position.set(5, 5, 10);
      scene.add(directionalLight);
      //
      // const directionalLight2 = new THREE.DirectionalLight(0xffffff, 1);
      // directionalLight2.position.set(0, 0, -10);
      // scene.add(directionalLight2);

      /*
      *  用两个 ambientLight   spotLight 组合 得到较好的结果
      * */

      const ambientLight = new THREE.AmbientLight(0xffffff, 0.8);
      ambientLight .position.set(0, 10, -10);
      scene.add(ambientLight);


      const spotLight = new THREE.SpotLight(0xffffff, 0.5);
      spotLight.position.set(0, 10, 0);
      spotLight.target.position.set(4, 4, 0);
      scene.add(spotLight);
      scene.add(spotLight.target);


      window.addEventListener('resize', handleResize);

    }

    const loadModel = () => {
      // Load the GLTF model
      const loader = new GLTFLoader();
      loader.load(
          "model/hero.glb",
          (gltf) => {
            // Add the model to the scene
            model = gltf.scene;
            console.log(model);

            scene.add(model);

            // 设置模型的位置
            model.position.set(0, 0, -0.3);
            // 设置模型的大小
            model.scale.set(1.2, 1.2, 1.2);

            // Render the scene
            render();
            animate();
          },
          (xhr) => {
            console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
          },
          (error) => {
            console.log("An error happened", error);
          }
      );
      const animate = () => {
        requestAnimationFrame(animate);
        if (model) {
          model.rotation.y -= 0.01;
        }
        renderer.render(scene, camera);
      };
    };


      onUnmounted(() => {
        window.removeEventListener('resize', handleResize);
      });

      const render = () => {
        renderer.render(scene, camera);
        requestAnimationFrame(render);
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

.container {
  overflow: hidden;
  }
</style>

<template>
  <hr>
  <h1>this is doc example page</h1>
  <div class="doc-container">
  </div>

</template>

<script>
import * as THREE from 'three';

export default {
  name: "docExample",
  mounted() {
    // once component is mounted, say hi to the console
    console.log("hi from doc example")

    const renderer = new THREE.WebGLRenderer();

    // const renderer_width = window.innerWidth/2;
    // const renderer_height = window.innerHeight/2;

    const renderer_width = 800;
    const renderer_height = 600;

    renderer.setSize(renderer_width , renderer_height );
    document.body.appendChild( renderer.domElement );
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
    const geometry = new THREE.BoxGeometry( 1, 1, 1 );
    const material = new THREE.MeshBasicMaterial( { color: 0x00ffff } );
    const cube = new THREE.Mesh( geometry, material );
    scene.add( cube );

    camera.position.z = 5;

    function animate() {
      requestAnimationFrame( animate );

      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;

      renderer.render( scene, camera );
    }

    animate();
  }
}
</script>

<style scoped>


/*.doc-container {*/

/*  background-color: #dc92fa;*/
/*  width: 500px; height: 500px;*/
/*  margin: 0 auto;*/
/*}*/
</style>

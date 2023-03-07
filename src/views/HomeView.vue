<template>
  <div class="home">

    <h1>HOME VIEW</h1>

   <!--
      native three js example
    -->
    <router-link to="/three-native" class="link">native three.js example </router-link>
    <br>

    <!--
       combine three.js with vue.js
     -->
    <router-link to="/three-vue" class="link">vue3.js & three.js example </router-link>
    <br>


    <!--
       load 3D file to the
     -->
    <router-link to="/three-load" class="link">load 3d files </router-link>
    <br>

    <!--
       using Add orbit controls
     -->
    <router-link to="/three-control" class="link">using Add orbit controls</router-link>





  </div>
</template>

<script>

export default {
  name: 'HomeView',
  components: {
    // HelloWorld,
  }
}

</script>

<style>
.link {
  text-decoration: none;
  font-size: 24px;
}
</style>

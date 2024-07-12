<template>
  <div class="flex h-auto">
    <div class="flex-1 bg-black">
      <SideBar></SideBar>
    </div>
    <div class="flex-2 bg-gray-800">
      <MiddleBar></MiddleBar>
    </div>
    <div class="flex-1 bg-black">
      <RightBar></RightBar>
    </div>
  </div>
</template>

<script>
import SideBar from './components/SideBar.vue';
import MiddleBar from './components/MiddleBar.vue';
import RightBar from './components/RightBar.vue';

export default {
  name: 'App',
  components: {
    SideBar,
    MiddleBar,
    RightBar,
  },
};
</script>

<style>
/* Add any additional styling here */
</style>

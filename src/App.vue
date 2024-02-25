<script setup>
import NSelect from './components/NSelect.vue';
</script>

<script>
// 导入MapViewLeaflet组件
import MapViewLeaflet from './components/MapViewLeaflet.vue'; // 根据实际路径修改

export default {
  name: 'App',
  components: {
    // 在components对象中注册MapViewLeaflet
    MapViewLeaflet
  }
};
</script>

<template>


  <main><MapViewLeaflet/>
  </main>

</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

main {
  width: 100vw; /* 视窗宽度 */
  height: 100vh; /* 视窗高度 */
  margin: 0;
  padding: 0;
  display: flex; /* 使用Flex布局确保子元素可以正确填充 */
  justify-content: center;
  align-items: center;
}

/* 如果MapView组件具有特定的类名或ID，可以直接针对它设置样式 */
/* 假设它没有特定的类名或ID，可以通过以下方式确保它填充<main>元素 */
main > * {
  width: 100%;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>

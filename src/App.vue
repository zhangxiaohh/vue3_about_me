<template>
  <div id="app-container">
    <nav class="main-nav">
      <router-link to="/">首页</router-link>
      <router-link to="/projects">项目</router-link>
      <router-link to="/experience">经历</router-link>
      <router-link to="/contact">联系</router-link>
      <router-link to="/details">个人博客</router-link>
    </nav>
    <main>
      <router-view />
    </main>
  </div>
</template>

<script setup lang="ts">
</script>

<style scoped>
#app-container {
  width: 100%;
  height: 100vh; /* 改为 100vh，固定容器高度 */
  display: flex;
  flex-direction: column;
  overflow: hidden; /* 防止出现外层滚动条 */
}

.main-nav {
  padding: 1.5rem;
  background: #1a1a1a;
  display: flex;
  justify-content: center;
  gap: 2rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.main-nav a {
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s;
}

.main-nav a:hover,
.main-nav a.router-link-active {
  color: #fff;
}

main {
  flex: 1;
  padding: 2rem;
  overflow: hidden; /* 防止主容器出现滚动条 */
}
</style>

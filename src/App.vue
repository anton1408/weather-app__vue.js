<template>
  <div id="app" class="ma-4">
    <div id="nav" class="ma-4">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
@import '@/design/index.scss';
</style>

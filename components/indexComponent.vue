<template>
  <div class="container_background">
    <div class="container_flex">
      <itemform-component />
      <itemlist-component />
    </div>
  </div>
</template>

<style scoped>
.container_background {
  background: rgba(255, 254, 251, 0.8);
  min-height: 100vh;
}
.container_flex {
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  gap: 16px;
  padding: 32px;
  max-width: 1376px;
}
</style>
